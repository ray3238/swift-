# MVVM 패턴 세부정리

------

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/24e8fa35-8298-48b5-a305-ba07b109b83c/Untitled.png)

> View

View에서는 단순히 유저 인터페이스를 표시하기 위한 로직만을 담당하고, 그 외에는 메소드 호출 정도만 있는 게 이상적

> ViewModel

기존의 UIKit을 import 할 필요도 없이 데이터 update 및 뷰 요소를 업데이트

> Model

데이터 구조를 지니고 있음

- MVVM의 장점
  - View - Model - ViewModel 모두 독립적으로 테스트가 가능하다.
- MVVM의 단점
  - 설계가 어렵다는 것과 뷰에 대한 처리가 복잡해지면 뷰모델도 거대해진다.

> MVVM 패턴의 사용 예시

## Model

API 를 받을 Model을 설계해본다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/6ac0a439-fefb-46d9-94ca-55a26d3ae26e/Untitled.png)

그 모델 안에는 struct들을 선언한다.

```swift
import Foundation

struct ArticleList: Decodable {
    let articles: [Article]
}

struct Article: Decodable {
    let title: String?
    let description: String?
}
// 우리가 받을 json 데이터 안에는 articles라는 array가 있다.
// 그것을 articleList로 받아줄 것이다.
// 우리가 필요로 하는 건 title, description 에 대한 정보뿐이므로
// article 하나에서는 필요로 하는 필드만 적어준다.
```

(나중에 값을 담을 struct들이 들어감)

## Service

MVVM에서 필수적이진 않지만 service를 하나 만들어서 API를 받아와본다.

로직은 VM(ViewModel)에 합쳐도 된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/f0846dba-8e2e-4a51-a0cf-8c574e8dff57/Untitled.png)

Service 안에는 API를 불러오는 코드를 작성 ( 밑의 코드는 URLSession으로 받아옴. )

```swift
import Foundation

class WebService {
    func getArticles(url: URL, completion: @escaping ([Article]?) -> ()) {

        URLSession.shared.dataTask(with: url) {
            (data, response, error) in
            if let error = error {
                print(error.localizedDescription)
                completion(nil) // if any error occurs, article can be nil
            }
            else if let data = data {
                let articleList = try? JSONDecoder().decode(ArticleList.self, from: data)
                print(articleList)
                if let articleList = articleList {
                    completion(articleList.articles)
                }
                print(articleList?.articles)

            }

        }.resume()

    }
}
```

(API를 불러오는 코드가 들어감)

@escaping 는 중요치 않다.

중요한 건 data를 받아오는지 독립적으로 확인할 수 있다는 것이다.

## **ViewModel**

ViewModel에서는 tableView가 기본적으로 필요로 하는 numberOfRowsInSection에 리턴해줄 함수와 cellForRowAt에 넣어줄 함수, 그리고 numberOfsection까지 정의 해줄 것이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/1d5b8580-dd2d-4a06-98ff-4e00187c2d9b/Untitled.png)

```swift
import Foundation

struct ArticleListViewModel {
    let articles: [Article]
}

extension ArticleListViewModel {
    var numberOfSections: Int {
        return 1
    }

    func numberOfRowsInSection(_ section: Int) -> Int {
        return self.articles.count
    }

    func articleAtIndex(_ index: Int) -> ArticleViewModel {
        let article = self.articles[index]
        return ArticleViewModel(article)
    }
}

struct ArticleViewModel {
    private let article: Article
}

extension ArticleViewModel {
    init(_ article: Article) {
        self.article = article
    }
}

extension ArticleViewModel {
    var title: String? {
        return self.article.title
    }
    var description: String? {
        return self.article.description
    }
}
```

(대충 struct, extension 등등이 들어감)

<aside> 🔥 (ViewModel에 정확히 어떤 애들이 들어가야 하는지, 무슨 이유로 그런 애들이 들어가게 됬는지는 아직 이해하지 못함) 더 찾아보기!

</aside>

## View(ViewController)

ViewController에서는 단순히 ViewModel에서 전달하는 데이터를 화면에 전달해주기만 할것이다.

이를 위해 TableViewCell과 TabelViewController를 만들어주었다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/12192ed3-9777-4d4d-b435-ab6a5fcd044d/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/4ee79f3f-2c53-4d3c-bba1-3580327aafb9/Untitled.png)

```swift
// TableViewController의 코드
import UIKit

class NewsListTableViewController: UITableViewController {

    private var articleListVM: ArticleListViewModel!

    override func viewDidLoad() {
        super.viewDidLoad()

        setup()
    }

    private func setup() {
        self.navigationController?.navigationBar.prefersLargeTitles = true

        let url = URL(string: "<https://newsapi.org/v2/top-headlines?country=us&apiKey=e9b514c39c5f456db8ed4ecb693b0040>")!
        WebService().getArticles(url: url) { //1
            (articles) in

            if let articles = articles {
                self.articleListVM = ArticleListViewModel(articles: articles) //2
            }
            DispatchQueue.main.async {
                self.tableView.reloadData()
            }
        }
    }
}

extension NewsListTableViewController {
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.articleListVM.numberOfRowsInSection(section)
    }

    override func numberOfSections(in tableView: UITableView) -> Int {
        return self.articleListVM == nil ? 0 : self.articleListVM.numberOfSections
    }

    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "ArticleTableViewCell", for: indexPath) as? ArticleTableViewCell
        else {fatalError("no matched articleTableViewCell identifier")}

        let articleVM = self.articleListVM.articleAtIndex(indexPath.row) //3 
        cell.descriptionLabel?.text = articleVM.description
        cell.titleLabel?.text = articleVM.title
        return cell
    }

}
// TableViewCell의 코드
import UIKit

class ArticleTableViewCell: UITableViewCell {

    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var descriptionLabel: UILabel!

    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)

        // Configure the view for the selected state
    }

}
```

이쪽에서 주목할 것은 작성한 모든 함수가 view에 호출되며, view에서는 단순히 함수 호출을 통해 만들어진 데이터를 받아쓸 뿐이라는 것.(한 마디로 ViewController부분, Model, ViewModel부분 서로간의 의존성이 적다는 것)