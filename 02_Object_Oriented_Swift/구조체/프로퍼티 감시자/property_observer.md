# 01 프로퍼티 감시자란?
**→ 프로퍼티 값이 변경될 때 원하는 동작을 수행할 수 있음**

# 02 기본 형태 및 예제
**프로퍼티 감시자의 기본 형태 ⬇️**
```SWIFT
var property: Type = 초기값 {
    willSet {
        // 값이 바뀌기 직전에 실행
    }
    
    didSet {
        // 값이 바뀐 직후에 실행
    }
}
```
\
\
⬇️⬇️ 예시 코드
```swift
struct Quokka {
    var mood: String = "행복🐔" {
        willSet {
            print("곧 \(newValue)로 바뀜 ")
        }
        didSet {
            print("이전: \(oldValue) → 현재: \(mood)")
        }
    }
}

var q = Quokka()
q.mood = "졸림Zz"
```
➡️➡️ 실행 흐름
```swift
곧 졸림으로 바뀜
이전: 행복🐔 → 현재: 졸림Zz
```
# 03 특징
- 연산 감시자에는 사용 ❌
- 감시자는 프로퍼티 값이 변경될 때 특정 동작을 수행할 수 있는 기능
- 값이 변경되기 직전에 `willSet`, 변경 직후에 `didSet`이 호출되며 둘 중 하나만 사용해도 됨 \
값이 기존과 같아도 항상 실행되며, `willSet`에선 변경될 값인 `newValue`, `didSet`에서는 이전 값인 `oldValue`를 사용할 수 ❌