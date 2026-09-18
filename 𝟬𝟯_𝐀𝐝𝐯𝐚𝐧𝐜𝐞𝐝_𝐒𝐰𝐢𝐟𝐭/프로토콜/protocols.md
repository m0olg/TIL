# 01 프로토콜
**이 기능은 꼭 구현해야 한다라고 정해놓은 규칙(약속)임** \
즉 어떤 타입이 프로토콜을 따르면 그 안에 있는 속성이나 함수를 반드시 만들어야함!! \
(타입에서 프로토콜의 요구사항을 충족 시키려면 프로토콜이 제시하는 청사진의 기능을 모두 구현해야함 = 프로토콜은 스스로 기능을 구현하지 ❌)
# 02 정의문법, 예제
### 정의문법 ⬇️
```swift
protocol 프로토콜 이름 {
    // 필수로 구현해야 하는 것들
}
```

### 예제 ⬇️⬇️
```swift
// 1. 프로토콜 만들기
protocol Eatable {
    func eat()
}

// 2. 하치와레 구조체가 프로토콜 채택
struct Hachiware : Eatable {
    func eat() {
        print("하치와레가 닭발을 먹어요🍗🐓")
    }
}

// 3. 사용하기
let hachiware = Hachiware()
hachiware.eat()
```
# 03 프로토콜 상속
프로토콜은 하나 이상의 프로토콜을 상속 받아 기존 프로토콜의 요구사항보다 더 많은 요구사항을 추가할 수 있음 \
프로토콜 상속 문법은 클래스의 상속 문법과 유사하지만, 프로토콜은 클래스와 다르게 다중상속이 ⭕️
```swift
protocol 자식프로토콜 : 부모프로토콜 {
    // 추가규칙
}
```
예제
```swift
// 1. 기본 프로토콜
protocol Eatable {
    func eat()
}

// 2. 상속받은 프로토콜
protocol CuteEatable: Eatable {
    func smile()
}

// 3. 하치와레가 채택
struct Hachiware: CuteEatable {

    func eat() {
        print("하치와레가 닭발을 냠냠")
    }

    func smile() {
        print("하치와레가 귀엽게 스마일 UwU")
    }
}
```
사용 ‼️
```swift
let h = Hachiware()
h.eat()
h.smlie()
```
### 클래스 상속과 프로토콜
클래스에서 상속과 프로토콜 채택을 동시에 하려면 상속 받으려는 클래스를 먼저 명시하고 그 뒤에 채택할 프로토콜 목록을 작성

# 04 프로토콜 준수 확인
`is`,`as` 연산자를 사용해서 인스턴스가 특정 프로토콜을 준수하는지 확인 가능
```swift
if 객체 is 프로토콜이름 {
    print("이 프로토콜을 따르고 있음")
}
```