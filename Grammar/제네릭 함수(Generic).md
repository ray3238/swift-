# :: 제네릭 함수(Generic) ::

------

- 제네릭(Generic)이란?
  - **제네릭이란 타입에 의존하지 않는 범용 코드를 작성할 때 사용**
  - **제네릭을 사용하면 중복을 피하고, 코드를 유연하게 작성할 수 있음**

애플 말에 따르면 Swift에서 가장 강력한 기능 중 하나로

Swift 표준 라이브러리의 대다수는 제네릭으로 선언되어 있음

- Array와 Dictoinary 또한 제네릭 타입

- **제네릭 함수(Generic Function)**

  ```swift
  func swapTwoInts(_ a: inout Int, _ b: inout Int) {
     let tempA = a
     a = b
     b = tempA
  }
  ```

  -**파라미터 모두 Int형일 경우엔 문제 없이 돌아감**

  -**파라미터 타입이 Double, String일 경우엔 사용할 수 없음**

  만약 DOuble, String에 대해서 swap 함수를 사용하고 싶다면

  ```swift
  func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
     let tempA = a
     a = b
     b = tempA
  }
  
  func swapTwoStrings(_ a: inout String, _ b: inout String) {
     let tempA = a
     a = b
     b = tempA
  }
  ```

  이런 식으로 사용할 수 있는데. 너무 복잡해보임

  이럴 때 사용하는 것이 제네릭

  제네릭의 코드는

  ```swift
  func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
     let tempA = a
     a = b
     b = tempA
  }
  ```

  이런식.

  꺽새<>를 이용해서 안에 타입처럼 사용할 이름(T)를 선언해주면,

  그 뒤로 해당 이름(T)를 타입처럼 사용할 수 있음.

  **T**를 **Type Parameter**라고 부르는데

  **T라는 새로운 형식이 생성되는 것이 아니라**,

  **실제 함수가 호출될 때 해당 매개변수의 타입으로 대체되는 Placeholder임**

  따라서 이렇게 swapTwoValues라는 함수를 제네릭으로 선언 해주면,

  ```swift
  var someInt = 1
  var aotherInt = 2
  swapTwoValues(&someInt,  &aotherInt)          // 함수 호출 시 T는 Int 타입으로 결정됨
   
   
  var someString = "Hi"
  var aotherString = "Bye"
  swapTwoValues(&someString, &aotherString)     // 함수 호출 시 T는 String 타입으로 결정됨
  ```

  **실제 함수를 호출할 때,**

  **Type Parameter인 T의 타입이 결정됨**

  ```swift
  swapTwoValues(&someInt, &aotherString)       // Cannot convert value of type 'String' to expected argument type 'Int'
  ```

  만약 서로 다른 타입을 파라미터로 전달하면,

  첫 번째 someInt를 통해 타입파라미터 T가 Int로 결정됐기 때문에,

  두 번째 파라미터인 therStrings의 타입이 Int가 아니라며 에러가 남

  **타입 파라미터는 굳이 T가 원하는 이름 마음대로 해도 되고,**

  **또 한개 말고 여러 개를 comma(,)를 이용해서 선언할 수도 있음**

  ```swift
  func swapTwoValues<One, Two> { ... }
  ```

  **T**나 **V** 같은 단일 문자, 혹은 **Upper Camel Case**를 사용한다고 함

  제네릭 함수를 사용했을 때의 장점은

  - **똑같은 내용의 함수를 오버로딩 할 필요 없이 제네릭을 사용하면 됨**
  - **따라서 코드 중복을 피하고 유연하게 코드를 짤 수 있음**