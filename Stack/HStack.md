## HStack
## 00 Stack은 어떨 때 쓸까
```swift
struct ContentVie: View {
    var body: some View {
        Text ("닭발")
            .padding() // 이걸 적용하지 않으면 텍스트 주위로 바짝 붙어서 여백이 없어짐
    }
}
```
-> View 프로토콜로 채택한 경우, 최상위 View 역할을 할 body라는 프로퍼티의 타입이 some View, 즉 불투명타입이라서 내부에서 View를 준수하고 있는 객체면 어떤 객체든 리턴이 ⭕ \
But, 단 한개의 View만 리턴을 해야됨


```swift
struct ContentView: View {
    var body: some View {
        Text("닭발엔 계란찜")
        Text("그리고 주먹밥")
    }
}
```
→ 이렇게 body 안에 두 개의 Text를 위처럼 써버리면 Text라는 두 개의 View를 리턴한다는 말이 됨 \
그래서 시뮬 돌릴 때 잘 나오지만 프리뷰가 쌍둥이 되는 문제가 있었음
.
.
<h4>따라서 !! <sub>만약 한 개 이상의 View를 리턴하고 싶을 경우, SiwftUI에선 이 View들을 하나로 컨테이너로 감싸주어야 함</sub></h4>

> 이 때 쓰는게 `Stack`과 `Group`임

근데 이 `Stack`에는 종류가 3개가 있어 이 3가지에 대해 설명을 해보겠다

---
---
# 01 HStack이란?
➜ 내부에 선언된 View들을 Leading에서 Trailing으로 배치하는 것

HStack을 풀면 'Horizontal Stack'임 \
즉, View를 수평으로 배치해주는 View임 (배치 방향은 Leading → Trailing 순)

위에서 텍스트 두개를 리턴해서 문제가 된 코드를 ⬇️⬇️
```swift
struct ContentView: View {
    var body: some View {
        HStack {
            Text("족발")
                .background(.yello)
            Text("닭발")
                .background(.red)
            Text("닭발과 족발")
                .background(.green)
        }
    }
}
```
이렇게 HStack으로 한번 감싸주기만 하면 됨

# 02 알아두면 좋은거
- `alignment` 세로 정렬 : HStack은 가로지만 세로 정렬 기준을 설정 가능 ‼️ \
```swift
HStack(alignment: .top) {
    Text ("짧은 글")
    Text ("긴 글\n줄바꿈됨!!")
}
```
- `Spacer` 빈 공간 채우기 \
```swift
HStack {
    Text("왼쪽")
    Spacer()
    Text("오른쪽")
}
```
결과 == `왼쪽 오른쪽`