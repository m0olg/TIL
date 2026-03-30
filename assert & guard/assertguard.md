# 01 `assert`와 `guard`
애플리케이션이 동작 도중에 생성하는 다양한 연산 결과값을 동적으로 확인하고 안전하게 처리할 수 있도록 확인하여 빠르게 처리할 수 있음
# 02 Assertion
`assert(_:_:file:line:)` 이라는 함수를 사용
- 이 함수는 디버깅 모드에서만 동작 -> 배포하는 애플리케이션에서는 제외
- 예상했던 조건의 검증을 위하여 사용
```swift
let age = -1

assert(age >= 0, "나이는 0 이상이어야 함!")
```
```swift
age가 -1 이라서 디버깅 모드에서 에러 발생
```
# 03 Guard
`guard`를 사용하여 잘못된 값의 전달 시 특정 실행구문을 빠르게 종료
- 디버깅 모드 뿐만 아니라 어떤 조건에서도 동작
- `guard`의 `else` 블럭 내부에는 특정 코드블럭을 종료하는 지시어 (`return`, `break` 등)가 필수임 ‼️
- 타입 캐스팅, 옵셔널과도 자주 사용함 \
이 외에도 단순 조건 판단 후 빠르게 종료할때도 용이
```swift
func printAge(age: Int?) {
    guard let age = age else {
        print("값 없음")
        return
    }
    
    print("나이:", age)
}
```
nil이면 바로 종료 / 아니면 아래 코드 실행