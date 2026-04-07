## ZStack
# 01 ZStack이란?
→ **내부에 선언된 View들을 겹처서 두 축으로 배치** \
쉽게 말해서 내가 보고 있는 화면의 방향을 뷰를 배치한다는 것


# 02 예제
```swift
struct ContentView: View {
    var body: some View {
        ZStack {
            Text("족발")
                .background(.yello)
            Text("닭발")
                .background(.red)
            Text("닭발오독오독")
                .background(.green)
        }
    }
}
```