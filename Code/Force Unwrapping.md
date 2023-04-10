# Force Unwrapping

------

<aside> 💡 옵셔널에 값이 들어있는지 아닌지 확인하지 않고 강제로 값을 꺼내는 방식

</aside>

만약 값이 없을경우(nil)런타임 오류가 발생. 추천 X.

```swift
var myName: String? = yagom
var youName: String! = nil

printName(myName!) // yagom
myName = nil

//print(myName!)
// 강제추출시 값이 없으므로 런타임 오류 발생
yourName = nil

//printName(yourName)
// nil 값이 전달되기 때문에 런타임 오류발생
```
