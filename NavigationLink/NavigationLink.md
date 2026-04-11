# 01 `NavigationLink`란?
→ **화면 이동을 위한 구성 요소로 `NacigationStack`안에서 사용되며 새로운 화면으로 `push`(쌓기) 동작을 통해 이동** ⭕️ \
화면 간 이동 구조를 자동으로 관리해주기 때문에 *뒤로가기* 버튼도 자동으로 제공 \
즉, 계층적인 화면 구조(목록 → 상세)를 쉽게 구성할 수 있게 해주는 기능

# 02 사용하는 이유?
그 다음 화면으로 이동하거나 이동한 이후에 전 화면으로 돌아가는 것과 같이 **유저가 앱을 사용하면서 다양한 화면 이동을 할 수 있도록 하기 위해**

# 03 예제
```swift
struct ContentView: View {
    var body: some View {
        
        NavigationView {
            
            NavigationLink("닭발만드는장면보러가기") {
                VStack {
                    Text("🐔🔪")
                        .font(.system(size:...))
                }
            }
            
        }
        
    }
}
```
어떻게 동작하냐면 ⬇️
- '닭발만드는장면보러가기'를 클릭하면
- 다음 View로 이동 (push)
- 그리고 자동으로 뒤로가기 버튼이 생김