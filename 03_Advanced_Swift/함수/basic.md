 # 함수의 기본
**Q. 함수란?** \
A. 특정 작업을 수행하는 코드 블록, 함수를 사용하면 코드의 재사용성과 가독성을 높일 수 있음

함수를 정의하는 기본 구조 ⬇️

```swift
func 함수이름(매개변수이름 : 매개변수타입) >> 반환타입 {
    // 함수가 수행할 작업
    return 반환값
}
```


\
(예제)
```swift
func makeChickenFeetMessage(menu: String) -> String {
    let message = "오늘의 야식은 (menu)~"
    return message
}

let todayMenu = makeChickenFeetMessage(menu: "닭발")
print(todayMenu) // == 오늘의 야식은 닭발~
```
\
= 매개변수가 없는 함수 (매개변수 : 함수에 넣어주는 값)
```swift
func todaymeun() -> String {
    return "닭발먹고싶다"
}

let menu = todayMenu()
print(menu) // == 닭발먹고싶다
```
\
= 매개변수와 반환값이 없는 함수
```swift
func cookChickenFeet() -> Void {
    print("국물닭발을요리하자")
    return
}

// 함수 구현이 짧은 경우 한 줄 표현
func waterChickenFeet() -> Void { print("국물 닭발 완성!") }


// 반환 타입(Void) 생략 가능
func serveChickenFeet() {
    print("닭발을주겟다")
    return
}

func eatChickenFeet() { print("닭발 냠냠") }
```