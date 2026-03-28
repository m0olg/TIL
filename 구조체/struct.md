# 구조체
→ **구조체는 swift에서 값을 저장하고 조작하는데 사용되는 사용자 정의 데이터 타입** \
구조체는 여러 속성(변수 또는 상수)과 메서드(함수)를 포함할 수 있음

구조체는 값 타입으로 타입 이름은 대문자 Camel Case를 사용하여 정의

**구조체 정의 하는 기본 문법 ⬇️**
```swift
struct 구조체이름 {
    // 속성 및 메서드 정의
}
```
구조체는 `struct` 키워드를 사용하여 정의 \
중괄호 `{}` 안에 속성과 메서드를 작성

예제 👀
```swift
struct Quokka {
    var name: String
    var age: Int
}

let quokka = Quokka(name: "퀔캌", age: 1)
print(quokka.name) // 출력: 퀔캌
print(quokka.age)  // 출력: 1
```
Quokka 구조체는 name과 age 속성을 가지며, quokka 인스턴스를 생성해 이름과 나이를 저장한 뒤 해당 값을 출력