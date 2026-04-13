# 01 `NavigationView`란?
→ SwiftUI의 가장 중요한 구성 요소 중 하나로 화면을 쉽게 psuh하고 Pop (화면을 쌓고, 화면을 빼서 뒤로) 할 수 있으며 \
 명확하고 계층적인 방식으로 정보 제공 가능

 # 02 있을 때와 없을 때
 ### ① `NavigationView`가 있을 때
 ```swift
NavigationView {
    Text("배고프다")
}
 ```
 == 화면 이동 구조를 쓸 수 있음
 ### ② `NavigationView`가 없을 때
 ```swift
 Text("배고프다")
 ```
 == 그냥 한 화면만 끝임

 # 03 사용하려면
 ```swift
 struct ContenView: View {
    var body: some View {
        NavigationView {
            Text("닭발먹고싶다")
        }
    }
 }
 ```
 ➡️ 이렇게 항목을 감싸야함

 원래 최상위에 위치해야 하지만 TabView 내에서 사용하는 경우는 **NavigationView가 TabView 안에** 있어야 함