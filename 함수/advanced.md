# 함수의 고급
### 매개변수 기본 값
→ **매개변수에 기본적으로 전달될 값을 미리 지정할 수 있음** \
( 기본값을 갖는 매개변수는 매개변수 목록 중 뒤쪽에 위치하는 게 좋음‼️ )
```swift
func hachiwareGreeting(name: String, message: String = "하치와레가 인사함 UwU") -> String {
    return "(message), (name)>_<"
}

print(hachiwareGreeting(name: "치이카와")) // == 하치와레가 인사함 UwU, 치이카와>_<
print(hachiwareGreeting(name: "우사기", message: "안녕")) // == 안녕, 우사기>_<
```


### 전달인자 레이블 
→ 함수를 호출할 때 함수 사용자의 입장에서 매개변수의 역할을 좀 더 명확하게 표현하고자 할 때 사용
```swift
func hachiwareIntroduce(to friend: String, from hachiware: String) {
    print("(friend) 안녕 나는 (hachiware)야")
}

hachiwareIntroduce(to: "치이카와", from: "하치와레") 
// == 치이카와 안녕 나는 하치와레야
```
### 가변 매개변수
→ 전달 받을 값의 개수를 알기 어려울 때 사용
```swift
func hachiware(points: Int...) -> Int {
    var total = 0
    for point in points {
        total += point
    }
    return total
}

print(hachiware(points: 1, 2, 3, 4, 5)) // == 15
```

----
### ++튜플 반환
함수는 튜플을 사용해 여러 데이터를 반환할 수 있음

**튜플이란?** \
→ 여러 값을 하나로 묶는 것으로 서로 다른 타입도 묶을 수 있음 ⭕️
```swift
func hachiwareSnackCalc(a: Int, b: Int) -> (sum: Int, total: Int) { // '하치와레스낵캘크' 라는 함수 선언, a, b 두 개의 정수 매개변수를 받고 sum : Int, total : Int) ... 튜프로 값 2개 변환
    let sum = a + b
    let total = a * b
    return (sum, total) // 합이랑 곱을 튜플로 묶어서 반환, (sum, total) ... 값 2개 동시에 리턴
}

let result = hachiwareSnackCalc(a: 3, b: 4)
print("하치와레 간식 합 (result.sum), 전체 세트 (result.total)") 
// == 하치와레 간식 합 7, 전체 세트 12
```
- `(sum : Int, total : Int)` → **튜플 변환**
- `result.sum` / `result.total` → 이름으로 꺼내기
- 값 2개를 한 번에 반환 가능 !!


\
\
**🐱 튜플을 쓰면 뭐가 좋을까??**
1. 값을 여러 개 한 번에 반환 가능 \
( 보통 함수는 값을 1개만 반환 하는데 튜플을 쓰면 합 + 곱 같이 반환 가능 )
2. 코드가 깔끔해짐 \
( 튜플을 안 쓰면 함수 2개 만들고 호출도 2번 해야함 )
3. 이름 붙여서 읽기 쉬움