# 01 상속이란?
**→ 클래스가 다른 클래스의 속성과 메서드를 물려받아 재사용할 수 있게 해주는 객체 지향 프로그래밍의 중요한 개념임** \
상속을 사용하면 코드의 재사용성을 높이고, 더 간결하고 유지보수하기 쉬운 코드를 작성할 수 있음

# 02 기본 구조와 예제
**⬇️⬇️ 기본 구조**
```swift
class 부모클래스 {
    // 속성
    // 메서드
}

class 자식클래스: 부모클래스 {
    // 추가 기능
}
```

\
**⬇️⬇️ 예시 코드**
```swift
// 부모 클래스
class Animal {
    var name: String = ""
    
    func eat() {
        print("밥 먹는 중")
    }
}

// 자식 클래스 (상속)
class Dog: Animal {
    func bark() {
        print("왈왈")
    }
}

// 사용
let dog = Dog()

dog.name = "달콤"  // 부모 속성 사용
dog.eat()         // 부모 메서드 사용
dog.bark()        // 자기 메서드 사용
```
Dog 클래스는 Animal을 상속받아 부모의 속성과 메서드를 그대로 사용하면서, bark() 같은 기능을 추가 ⭕
# 03 재정의
→ 부모 클래스가 가지고 있는 기능(프로퍼티/메서드)을 자식 클래스에서 다르게 다시 정의하는 것
## 3-1 속성 재정의
→ 서브 클래스는 슈퍼 클래스의 속성을 재정의하여 자신만의 구현을 제공할 수 있음 \
계산 속성을 재정의 할 때는 `override` 키워드 사용
```swift
class Animal {
    var name: String {
        return "동물중에가장귀여운건"
    }
}

class Dog: Animal {
    override var name: String {
        return "강아지"
    }
}

let dog = Dog()
print(dog.name)
```
```swift
강아지
```
🗨️ 부모 프로퍼티를 다르게 계산하도록 바꾼 것
## 3-2 메서드 재정의
→ 서브 클래스는 슈퍼 클래스의 메서드를 재정의하여 자신만의 구현을 제공, 메서드를 재정의 하려면 `override` 키워드 사용
```swift
class Animal {
    func speak() {
        print("병아리삐약삐약강아지는?")
    }
}

class Dog: Animal {
    override func speak() {
        print("멍멍")
    }
}

let dog = Dog()
dog.speak()
```
```swift
== 멍멍
```
🗨️ 부모 메서드를 자식이 덮어씀
# 04 super 키워드
→ `super` 키워드는 서브 클래스에서 슈퍼 클래스의 속성과 메서드를 참초할 때 사용, `super`를 사용하여 슈퍼 클래스의 메서드를 호출하거나 속성에 접근 가능
```swift
class Animal {
    func speak() {
        print("소는음메강아지는")
    }
}

class Dog: Animal {
    override func speak() {
        super.speak()   // 부모 메서드 실행
        print("멍멍")
    }
}

let dog = Dog()
dog.speak()
```
```swift
소는음메강아지는
멍멍
```
🗨️ 부모 거 먼저 실행하고 추가 행동
---
### **⭐ 요약 ⭐**
- **상속** : 클래스가 다른 클래스의 속성과 메서드를 물려받아 재사용할 수 있게 해주는 것
- **재정의** : 서브 클래스에서 슈퍼 클래스의 계산 속성 및 메서드를 재정의할 때 `ocerride` 키워드를 사용
- **`super` 키워드** : 서브 클래스에서 슈퍼 클래스의 속성과 메서드를 참조할 때 사용


\
⬇️⬇️⬇️
| 개념      | 설명                |
| ------- | ----------------- |
| 메서드 재정의 | 부모 함수 내용을 바꾼다     |
| 속성 재정의  | 프로퍼티 값을 새 방식으로 제공 |
| super   | 부모 기능을 가져와서 같이 사용 |
