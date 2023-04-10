# Optional Bindding

<aside> 💡 1. 옵셔널의 값을 꺼내오는 방법 중 하나

1. nil 체크 + 안전한 값 추출
2. 옵셔널 안에 값이 들어있는지 확인하고 값이 있으면 값을 꺼내옴
3. if-let 방식 사용 </aside>

```swift
func printName(_ name: String) {
    print(name)
}

var myName: String? = nil

//printName(myName)
// 전달되는 값의 타입이 다르기 때문에 컴파일 오류발생

if let name: String = myName {
    printName(name)
} else {
    print("myName == nil")
}

var yourName: String! = nil

if let name: String = yourName {
    printName(name)
} else {
    print("yourName == nil")
}

// name 상수는 if-let 구문 내에서만 사용가능합니다
// 상수 사용범위를 벗어났기 때문에 컴파일 오류 발생
//printName(name)
```

```swift
// ,를 사용해 한 번에 여러 옵셔널을 바인딩 할 수 있습니다
// 모든 옵셔널에 값이 있을 때만 동작합니다
myName = "yagom"
yourName = nil

if let name = myName, let friend = yourName {
    print("\\(name) and \\(friend)")
}
// yourName이 nil이기 때문에 실행되지 않습니다
yourName = "hana"

if let name = myName, let friend = yourName {
    print("\\(name) and \\(friend)")
}
// yagom and hana
```
