## VStack
# 01 VStack이란?
#### → **내부에 선언된 View들을 Top에서 Bottom으로 배치**

VStack을 풀면 'Vertical Stack'임 \
즉, View 수직으로 배치해주는 View임 (배치 방향은 Top → Bottom 순)

VStack으로 감싸주면 내가 내부에 선언해준 Text를 위에서 아래로 순서대로 수직을 배치 해줌

# 02 예제
```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Text("족발")
                .background(.yello)
            Text("닭발")
                .background(.red)
            Text("족발보단닭발이훨씬맛있다")
                .background(.green)
        }
    }
}
```