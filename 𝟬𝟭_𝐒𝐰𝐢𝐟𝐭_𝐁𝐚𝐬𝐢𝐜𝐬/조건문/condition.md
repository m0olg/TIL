# 01 if-else 구문
→ `if`만 단독적으로 사용해도 되고, `else if, else`와 조합해서 사용 가능 \
`if` 뒤의 조건 값에는 `Bool` (True, false) 타입의 값만 위치해야하고 조건 값을 감싸는 소괄호는 선택 사항!

⬇️⬇️ **형식**
```swift
if 조건식 {
    조건식에 만족한다면 (true라면) 해당 구문 실행
} else {
    아니라면 else 구문 실행
}
```

```swift
// 치이카와의 나이를 구별하는 if문
let Chiikawaage = 20

if Chiikawaage < 20 {
    print("다 큰 치이카와 UwU")
} else {
    print("아직 애기인 치이카와")
}
```

### ① 비교 조건이 많은 경우 (`elif`사용)

⬇️⬇️ **형식**
```swift
if 조건식 1 {
    조건식 1이 만족하면 해당 구문 출력
} else if 조건식 2 {
    조건식 2를 만족하면 해당 구문 출력
} else {
    둘 다 아니라면 해당 구문 출력
}
```



### ② 조건에 자주 쓰이는 비교와 논리 연산자

**01 비교 연산자 (조건에 사용)**
- `>` 크다
- `<` 작다
- `>=` 크거나 같다 (이상)
- `<=` 작거나 같다 (이하)
- `==` 같다
- `!=` 다르다

\
**02 논리 연산자 (조건이 여러 개)**
- `&&` 그리고 (AND)
- `||` 또는 (OR)

\
예시 ⬇️
```swift
let hungry = true
let snack = 3

if hungry && snack > 0 {
    print("하치와레 간식 냠냠")
}
```

# 02 switch 구문
→ if조건 과는 달리 패턴 기반으로 사용, swift의 switch 구문은 다른 언어에 비해 강력한 힘을 발휘 \
정수타입의 값만 비교하는 것이 아닌 스위프트 기본 타입을 지원하며 다양한 패턴과도 응용이 가능
```swift
let menu = "닭발"

switch menu {
case "닭발":
    print("하치와레: 닭발 먹자ㅎ.ㅎ")
case "라면":
    print("하치와레: 라면 먹자 UwU")
default:
    print("하치와레: 다른 거 먹자 ^!^")
}
// == 하치와레 : 닭발 먹자ㅎ.ㅎ
```
 - 각각의 case 내부에는 실행 가능한 코드가 반드시 위치 해야함
 - 매우 한정적인 값이 비교값이 아닌 한 `default` 구문은 반드시 작성
 - 명시적 `ㅇbreak`를 하지 않아도 자동으로 case마다 `break` 됨
 - fallthrough 키워드를 사용하여 break를 무시 ⭕
 - 쉼표를 사용하여 하나의 case에 여러 패턴을 명시 ⭕

 > where 절을 사용해서 추가적인 조건을 지정할 수 있음

 ```swift
 let score = 85 // 상수 score에 85 저장

switch score {
case let x where x >= 90: // score 값을 x에 저장, 조건 x>=90인가? 85>=90 이므로 거짓, 다음 case로 넘어감
    print("A")
case let x where x >= 80: // 이 구문은 참이므로 이 case 실행
    print("B")
case let x where x >= 70:
    print("C")
default:
    print("F")
}
// == B (2번째 구문이 참이므로 B 출력)
```