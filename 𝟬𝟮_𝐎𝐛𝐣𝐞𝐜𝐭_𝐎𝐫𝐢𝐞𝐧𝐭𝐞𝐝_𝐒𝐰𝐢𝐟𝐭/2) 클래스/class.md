# 클래스
**→ 클래스는 값을 저장하고 조작하는 데 사용되는 사용자 정의 데이터 타입**

`class`는 참조 타입으로 타입 이름은 대문자 Camel Case를 사용하여 정의


**클래스 정의 하는 기본 문법 ⬇️**
```SWFIT
class 클래스이름 {
    // 속성 및 메서드 정의
}
```
**➡ 특징** \
    - 구조체와 매우 유사 \
    - 구조체는 값 타입, 클래스는 참조 타입 \
    - 클래스는 다중 상속이 되지 않음

**타입 메서드**
- 재정의 불가 타입 메서드 - static
- 재정의 가능 타입 메서드 - class
---
### 기본 이니셜라이저
→ 클래스의 모든 속성이 기본값을 가지면, Swift는 자동으로 기본 이니셜라이저를 생성, \
기본 이니셜라이저는 각 속성에 대해 초기값을 설정할 수 있도록 함
### 커스텀 이니셜라이저
→ 클래스의 초기화 과정을 사용자 정의할 수 있음 \
이를 통해 초기화 시 추가적인 로직을 구현하거나, 기본 이니셜라이저와는 다른 방식으로 초기화를 수행
### 디이니셜라이저
→ 클래스는 인스턴스가 메모리에서 해제되기 직전에 호출되는 디이니셜라이저를 가질 수 있음 \
디이니셜라이저는 `deinit` 키워드를 사용하여 정의

```swift
// Quokka 클래스
class Quokka {
    var name: String = "기본 쿼카"  // 기본값 있음
    var age: Int = 0               // 기본값 있음
}

// 기본 이니셜라이저 사용
let q1 = Quokka()
print(q1.name) // 기본 쿼카
print(q1.age)  // 0

// --------------------------------------

// 커스텀 이니셜라이저
class QuokkaCustom {
    var name: String
    var age: Int
    var favoriteFood: String

    // 내가 직접 초기화 방식 정의
    init(name: String, age: Int) {
        self.name = name
        self.age = age
        
        // 딸기라떼 좋아하는 쿼카 🐹🐹🐹🐹
        self.favoriteFood = "딸기라떼"
    }
}

// 커스텀 초기화로 생성
let q2 = QuokkaCustom(name: "퀔캌", age: 3)
print(q2.name)          // 퀔캌
print(q2.favoriteFood)  // 딸기라떼

// --------------------------------------


// 디이니셜라이저
class QuokkaDeinit {
    var name: String

    init(name: String) {
        self.name = name
        print("\(name) 생성됨")
    }

    deinit {
        // 메모리에서 사라지기 직전에 호출됨
        print("\(name) 사라짐")
    }
}

// 실행 예시
var q3: QuokkaDeinit? = QuokkaDeinit(name: "카피바라 친구 쿼카")
q3 = nil // 여기서 deinit 호출됨
```