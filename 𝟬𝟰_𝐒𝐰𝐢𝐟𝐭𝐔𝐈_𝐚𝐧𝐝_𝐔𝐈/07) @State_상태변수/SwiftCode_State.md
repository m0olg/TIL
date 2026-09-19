## 코드 분석 (20260920-보충)

<br>

# 01 전체 코드
```swift
import SwiftUI

struct StateExample: View {
    @State private var count = 0

    var body: some View {
        VStack(spacing: 16) {
            Text("먼작귀가 \(count)마리 있음")

            Button("먼작귀 추가") {
                count += 1
            }
        }
        .padding()
    }
}
```

<br>
<br>


# 02 코드 분석

### 2-1 `@State private var count = 0`
→ 먼작귀의 마릿수를 저장하는 상태 변수

- 처음에는 `count`가 `0`
- `@State`가 붙어 있어서 값의 변화를 감지함
- 값이 바뀌면 화면도 자동으로 업데이트됨

### 2-2 `Text("먼작귀가 \(count)마리 있음")`
→ 현재 `count` 값을 화면에 보여줌

- `\(count)`는 문자열 안에 `count` 값을 넣는 문법
- 처음에는 `먼작귀가 0마리 있어요`라고 표시됨

### 2-3 버튼을 누르면 +1
```swift
Button("먼작귀 추가") {
    count += 1
}
```
→ 버튼을 누를 때마다 `count`가 1씩 증가함

```
0 → 1 → 2 → 3
```

<br>
<br>


# 03 실행 과정
- 버튼 클릭
- `count` 값이 1 증가
- `@State`가 값 변경을 감지
- `Text` 내용이 변경됨
- 화면이 자동으로 다시 그려짐



### ‼️ 정리하자면

> `@State`로 선언한 `count`가 바뀌면 `"먼작귀가 \(count)마리 있어요"`도 자동으로 바뀜