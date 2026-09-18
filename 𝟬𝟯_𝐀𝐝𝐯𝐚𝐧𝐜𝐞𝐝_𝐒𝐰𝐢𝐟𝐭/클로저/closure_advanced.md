### 클로저 고급
클로저 매개변수를 갖는 함수 `calculate(a:b:method:)`와 결과값을 저장할 함수 `result`를 먼저 선언
# 01 후행 클로저
→ 클로저가 함수의 마지막 전달인자라면 마지막 매개변수 이름을 생략한 후 함수 소괄호 외부에 클로저를 구현 ⭕
```swift
func doSomething(action: () -> Void) {
    action()
}

// 괄호 밖으로 빼기 (후행 클로저)
doSomething {
    print("쿼카 등장 뿅")
}
```
# 02 반환타입 생략
→ `calculate(a:b:method:)` 함수의 `method` 매개변수는 `Int` 타입을 반환할 것이라는 사실을 컴파일러도 알기 때문에 클로저에서 반환타입을 명시해 주지 않아도 됨!!
```swift
let add = { (a: Int, b: Int) in
    a + b
}

print(add(2, 3)) // 5
```
(대신 in 키워드는 생략 불가)
# 03 단축 인자이름
→ 클로저의 매개변수 이름이 굳이 불필요하다면 단축 인자 이름을 활용할 수 있음 \
단축 인자 이름은 클로저의 매개변수의 순서대로 `0$`, `1$`, `$2`... 처럼 표현
```swift
let add = { $0 + $1 }

print(add(2, 3)) // 5
```

# 04 암시적 반환 표현
→ 반환하는 값이 있다면 클로저의 마지막 줄 결과값은 암시적으로 반환값으로 취급
```swift
let multiply = { (a: Int, b: Int) in
    a * b  // return 생략
}

print(multiply(3, 4)) // 12
```
# 05 축약 전과 후 비교
⬇️⬇️
```swift
// 축약 전
let add = { (a: Int, b: Int) -> Int in
    return a + b
}

// 축약 후
let addShort = { (a: Int, b: Int) in
    a + b
}
```
불필요한 `-> Int`와 `return`을 생략해서 더 간결하게 만듦