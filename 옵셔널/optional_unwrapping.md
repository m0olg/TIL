# 02 옵셔널 값 추출
- **옵셔널을 꺼내는 방법**
     1. Optional Binding - 옵셔널 바인딩
    → 상자에다가 값이 있냐 물어보고 값이 있으면 꺼내고 없으면 지나침
    2. Force Unwrapping - 강제 추출
    → 옵셔널의 값을 강제로 추출❗


| 방법      | 코드                       | 동작       | 결과           | 안전성  |
| ------- | ------------------------ | -------- | ------------ | ---- |
| 옵셔널 바인딩 (Optional Binding) | `if let c = capybara {}` | 값 있으면 꺼냄 | 없으면 실행 안됨    | 안전 |
| 강제 언래핑 (Force Unwrapping)  | `capybara!`              | 무조건 꺼냄   | nil이면 크래시 | 위험 |

### 옵셔널 바인딩 (`if let`)
```swift
var capybara: String? = "카피바라"

if let c = capybara {
    print("꺼낸 값:", c)
} else {
    print("값이 없음")
}
// == 꺼낸 값: 카피바라
```
---
```swift
var capybara: String? = nil

if let c = capybara {
    print("꺼낸 값:", c)
} else {
    print("값이 없음")
}
// == 값이 없음
```


### 강제 언래핑 (`Force Unwrapping`)
```swift
var capybara: String? = "카피바라"

print(capybara!) // 안전 (값 있음)
// == 카피바라
```
---

```swift
var capybara: String? = nil

print(capybara!) // 런타임 에러 발생
// == Fatal error: Unexpectedly found nil...
// 왜 저렇게 뜨냐면 !해서 강제로 꺼냈는데 비어있으므로 '값이 없는데 왜 꺼내냐'고 하는 것임
```