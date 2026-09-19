# 01 옵셔널 체이닝
이름에서 알 수 있듯이 체인을 이용하여 옵셔널에 접근 하는 것으로 연속적인 옵셔널들을 연쇄적으로 확인 해보는 것 ‼️
## 1-1 종류
- `Force unwrapping` : !를 써서 강제로 옵셔널 추출
- `Optional Binding` : `if let`, `guard let`을 써서 옵셔널 추출
- `Optional Chaining` : 체인의 형태처럼 연쇄적으로 옵셔널에 접근
#### 옵셔널 바인딩 (if let)
```swift
var name: String? = "하치와레"

if let unwrapped = name {
    print(unwrapped) // 값이 있으면 출력
} else {
    print("값 없음")
}
```
## 1-2 옵셔널 바인딩 + 옵셔널 체이닝
이 두 개를 같이 쓰면 옵셔널을 추출할 수 있음!
```swift
class CatDog {
    var name: String?
}

var catdog: CatDog? = CatDog()
cat?.name = "하치와레"

if let name = catdog?.name {
    print(name)
}
```

# 02 nil 병합 연산자
Swift의 nil 값의 경우에는 Objective-C 객체의 부재를 나타내고 쉽게 말해서 단지 특정 타입에 대한 값의 부재를 보여주는 것임
```swift
var name: String? = nil

let result = name ?? "기본값"
print(result) // "기본값"
```