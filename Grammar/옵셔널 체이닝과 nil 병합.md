# :: ***\*옵셔널 체이닝과 nil 병합\**** ::

------

- **옵셔널 체이닝**

  - 옵셔널 체이닝은 옵셔널의 **내부의 내부의 내부로 옵셔널이 연결되어 있을 때** 유용하게 활용할 수 있음
  - 매번 **nil** 확인을 하지 않고 **최종적으로 원하는 값**이 있는지 없는지 확인할 수 있음

  ```swift
  // 예제 클래스
  // 사람 클래스
  class Person {
      var name: String
      var job: String?
      var home: Apartment?
      
      init(name: String) {
          self.name = name
      }
  }
  // 사람이 사는 집 클래스
  class Apartment {
      var buildingNumber: String
      var roomNumber: String
      var `guard`: Person?
      var owner: Person?
      
      init(dong: String, ho: String) {
          buildingNumber = dong
          roomNumber = ho
      }
  }
  
  // 옵셔널 체이닝 사용
  let yagom: Person? = Person(name: "yagom")
  let apart: Apartment? = Apartment(dong: "101", ho: "202")
  let superman: Person? = Person(name: "superman")
  
  // 옵셔널 체이닝이 실행 후 결과값이 nil일 수 있으므로
  // 결과 타입도 옵셔널입니다
  
  // 만약 우리집의 경비원의 직업이 궁금하다면..?
  
  // 옵셔널 체이닝을 사용하지 않는다면...
  func guardJob(owner: Person?) {
      if let owner = owner {
          if let home = owner.home {
              if let `guard` = home.guard {
                  if let guardJob = `guard`.job {
                      print("우리집 경비원의 직업은 \\(guardJob)입니다")
                  } else {
                      print("우리집 경비원은 직업이 없어요")
                  }
              }
          }
      }
  }
  
  guardJob(owner: yagom)
  
  // 옵셔널 체이닝을 사용한다면
  func guardJobWithOptionalChaining(owner: Person?) {
      if let guardJob = owner?.home?.guard?.job {
          print("우리집 경비원의 직업은 \\(guardJob)입니다")
      } else {
          print("우리집 경비원은 직업이 없어요")
      }
  }
  
  guardJobWithOptionalChaining(owner: yagom)
  // 우리집 경비원은 직업이 없어요
  
  yagom?.home?.guard?.job // nil
  
  yagom?.home = apart
  
  yagom?.home // Optional(Apartment)
  yagom?.home?.guard // nil
  
  // 경비원 할당
  yagom?.home?.guard = superman
  
  yagom?.home?.guard // Optional(Person)
  
  yagom?.home?.guard?.name // superman
  yagom?.home?.guard?.job // nil
  
  yagom?.home?.guard?.job = "경비원"
  ```

- **nil 병합 연산자**