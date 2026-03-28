# 열거형
→ **정해진 선택지 중 하나만 선택하는 타입** (다른 언어의 열거형과는 많이 다름)

**열거형 정의 하는 기본 문법 ⬇️**
```swift
enum 이름 {
	case 이름1
	case 이름2
	case 이름3, 이름4, 이름5
	// ...
}
```
- `enum`은 타입이므로 대문자 Camel Case를 사용하여 이름 정의
- 각 case는 소문자 Camel Case로 정의
- 각 case는 그 자체가 고유의 값
- 각 case는 한 줄에 개별로도 한 줄에 여러개도 정의 ⭕

**예제 ⬇️**
```swift
enum QuokkaMood {
    case happy
    case sleepy
    case hungry
}

var mood: QuokkaMood = .sleepy

switch mood {
case .happy:
    print("쿼카 기분 조음 🐔🦶 ")
case .sleepy:
    print("쿼카 졸림 ZZ")
case .hungry:
    print("쿼카 배고픔 ㅠㅠ")
}
```

---

## 1) 원시값
→ **C 언어의 `enum`처럼 정수값을 가질 수도 있음 rawValue를 사용**하면 됨 \
case 별로 각각 다른 값을 가져야함

⬇️⬇️
```swift
enum QuokkaLevel: Int {
    case baby = 1
    case adult = 2
}

let level = QuokkaLevel.adult
print(level.rawValue) // 2
```
각 case에 숫자 같은 값을 직접 연결해서 사용할 수 있음

### 원시값을 통한 초기화
**rawValue를 통해 초기화 할 수 있음** \
rawValue가 case에 해당하지 않을 수 있으므로 rawValue를 통해 초기화 한 인스턴스는 옵셔널 타입‼️

⬇️⬇️
```swift
enum QuokkaLevel: Int {
    case baby = 1
    case adult = 2
}

// 숫자로 enum 만들기
let level = QuokkaLevel(rawValue: 1)

print(level) // Optional(QuokkaLevel.baby)
```
원시값을 이용해 enum을 생성할 수 있으며, 값이 맞지 않으면 nil이 될 수 있어 Optional로 반환

## 2) 메서드
→ 스위프트의 열거형에는 메서드도 추가 ⭕
⬇️⬇️
```swift
enum QuokkaMood {
    case happy
    case sleepy
    case hungry
    
    // 👉 메서드 정의
    func description() -> String {
        switch self {
        case .happy:
            return "쿼카 기분 조음 🐔🦶"
        case .sleepy:
            return "쿼카 졸림 zz"
        case .hungry:
            return "쿼카 배고픔 ㅠㅠ"
        }
    }
}

// 사용
let mood = QuokkaMood.happy
print(mood.description())
```
열거형 안에서도 메서드를 정의할 수 있으며, 각 케이스에 따라 다른 동작을 수행 ⭕