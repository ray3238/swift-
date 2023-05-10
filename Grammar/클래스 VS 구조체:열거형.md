# :: 클래스 vs 구조체 / 열거형 ::

------

|          | Class           | Struct    | Enum      |
| -------- | --------------- | --------- | --------- |
| 타입     | 참조(Reference) | 값(Value) | 값(Value) |
| 상속     | O               | X         | X         |
| 익스텐션 | O               | O         | O         |

- 구조체는 언제 사용하나?
  - 연관된 몇몇의 값들을 모아서 하나의 데이터타입으로 표현하고 싶을 때
  - 다른 객체 또는 함수 등으로 전달될 때 **참조가 아닌 복제를 원할 때**
  - 자신을 상속할 필요가 없거나, 자신이 다른 타입을 **상속받을 필요가 없을 때**
  - Apple 프레임워크에서 프로그래밍을 할 때에는 주로 클래스를 많이 사용

------

## Value(값) VS Reference(참조)

- Value

  - 데이터를 전달할 때 **값을 복사하여 전달**

- Reference

  - 데이터를 전달할 때 **값의 메모리 위치를 전달**

- 참고용 코드

  ```swift
  struct ValueType {
      var property = 1
  }
  
  class ReferenceType {
      var property = 1
  }
  
  // 첫 번째 구조체 인스턴스
  let firstStructInstance = ValueType()
  
  // 두 번째 구조체 인스턴스에 첫 번째 인스턴스 값 복사
  var secondStructInstance = firstStructInstance
  
  // 두 번째 구조체 인스턴스 프로퍼티 값 수정
  secondStructInstance.property = 2
  
  // 두 번째 구조체 인스턴스는 첫 번째 구조체를 똑같이 복사한 
  // 별도의 인스턴스이기 때문에 
  // 두 번째 구조체 인스턴스의 프로퍼티 값을 변경해도
  // 첫 번째 구조체 인스턴스의 프로퍼티 값에는 영향이 없음
  print("first struct instance property : \\(firstStructInstance.property)")    // 1
  print("second struct instance property : \\(secondStructInstance.property)")  // 2
  
  // 클래스 인스턴스 생성 후 첫 번째 참조 생성
  let firstClassReference = ReferenceType()
  // 두 번째 참조 변수에 첫 번째 참조 할당
  let secondClassReference = firstClassReference
  secondClassReference.property = 2
  
  // 두 번째 클래스 참조는 첫 번째 클래스 인스턴스를 참조하기 때문에
  // 두 번째 참조를 통해 인스턴스의 프로퍼티 값을 변경하면
  // 첫 번째 클래스 인스턴스의 프로퍼티 값을 변경하게 됨
  print("first class reference property : \\(firstClassReference.property)")    // 2
  print("second class reference property : \\(secondClassReference.property)")  // 2
  ```

  - Swift의 DataType

    ```swift
    //Swift의 DataType은 전부 구조체로 구현되어 있음
    public struct Int
    
    public struct Double
    
    public struct String
    
    public struct Dictionary<Key : Hashable, Value>
    
    public struct Array<Element>
    
    public struct Set<Element : Hashable>
    ```

  - Swift는 구조체를 선호

    - Swift는 구조체, 열거형 사용을 선호
    - Apple 프레임워크는 대부분 class 사용
    - Apple 프레임워크 사용시 struct/class 선택은 우리의 몫