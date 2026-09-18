# 문자열 보간법
→ 프로그램 실행 중 문자열 내에 변수 or 상수의 실질적인 값을 표현하기 위해 사용함
→ \ ()

```swift
let name = "Chicken feet"
print("내 이름은 \(name)입니다")
```

⬇️⬇️ 계산도 가능함
```swift
print("2 + 3 = \(2 + 3)"
// 출력 == 2 + 3 = 5
```
.

.


.


콘솔로그랑 문자열 보간이랑 같이 사용! 

```swift
let name = "Chicken feet" // let으로 상수 선언
let age = 100 // let으로 상수

print("이름: \(name), 나이: \(age)")
// print를 이용해 상수에 저장된 "Chicken feet"과 나이 "100"을 출력
```