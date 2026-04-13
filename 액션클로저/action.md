# 01 액션 클로저란?
→ 스위프트UI는 선언형이므로 '무슨 일이 일어나면 어떤 코드를 실행할 지'를 클로저를 통해 전달함

# 02 예제
```swift
Button(action: {
    print("눌림")
}) {
    Text("뿅")
}
```
- `action:` 뒤의 { ... }이 액션 클로저임
- 버튼이 눌릴 때 실행

# 03 활용
```swift
Button(action: deleteItem) {
    Text("삭제")
}
```
- 함수 자체를 넘길 수도 있음


\
\
### ⬇️⬇️ 상태 클로저(`@State`)와 함께 쓰면
```swift
@State private var count = 0

Button("증가") {
    count += 1
}
```
- 버튼 클릭
- 액션 클로저 실행
- 상태 변경
- UI 자동 업데이트