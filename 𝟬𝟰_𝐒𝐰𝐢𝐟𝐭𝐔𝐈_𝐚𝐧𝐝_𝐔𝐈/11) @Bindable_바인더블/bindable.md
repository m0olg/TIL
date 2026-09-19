## 바인더블
# 01 `Bindable`이란?
→ SwiftUI에서 값이 양방향으로 연결되도록 해주는 기능 즉, 부모 뷰의 값을 자식 뷰가 읽고 수정할 수 있게 해줌

나온지 얼마 안 된 기능이고 바인딩하고 비슷하면서도 다름

# 02 예제 및 코드 해석
```swift
struct ParentView: View {
    @State private var name = ""

    var body: some View {
        ChildView(name: $name)
    }
}

struct ChildView: View {
    @Binding var name: String

    var body: some View {
        TextField("이름", text: $name)
    }
}
```

<details open>
<summary>==== 코드 해석 !!!!!! ==== </summary>

### ① `@State private var name = ""`
* `ParentView`가 실제 값을 소유함
* 값이 변경되면 화면도 다시 업데이트됨
### ② `ChildView(name: $name)`
* $name은 name의 Binding을 전달함
* 자식 뷰가 값을 직접 수정할 수 있게 됨
### ③ `@Binding var name: String`
* 자식 뷰에서 부모의 `name` 값을 연결해서 사용
### ④ `TextField("이름", text: $name)`
* 사용자가 입력한 값이 `name`에 바로 반영

> ### 쉽게 비유하면
* `@State` : 부모가 가진 원본 데이터
* `@Binding` : 그 데이터를 수정할 수 있는 연결 통로
* `$name` : `name`의 연결 통로를 전달하는 표현

</details>

# 03 언제 사용할까?
자식 뷰가 부모 뷰의 값을 변경해야 할 때 사용
```swift
struct ToggleView: View {
    @Binding var isOn: Bool

    var body: some View {
        Toggle("알림", isOn: $isOn)
    }
}
```
> #### 부모 뷰 ⬇️⬇️
```swift
struct ContentView: View {
    @State private var isOn = false

    var body: some View {
        ToggleView(isOn: $isOn)
    }
}
```
`ToggleView`에서 값을 변경하면 부모의 `isOn`도 함께 변경

# 04 주의할 점
`@Binding`은 값을 직접 소유하지 않음


부모 뷰에 있는 값의 참조만 전달받는 것 !!!!

따라서 보통 부모에는 `@State`가 있고 자식에는 `@Binding`이 있따

> **부모** : `@State`
> **자식** : `@Binding` (바인더블)

# 05 `@Binding`과의 차이점
* @Binding: 부모 뷰의 값을 자식 뷰에서 수정하도록 연결
* @Bindable: 객체의 속성을 바인딩할 수 있도록 만들어줌

```swift
@Binding var isOn: Bool
```
→ `isOn` 값 하나를 연결

```swift
@Bindable var user: User
```
→ `user.name`처럼 객체 안의 속성을 연결

#### 비유하면 🍀
↳ `@Binding` : <mark>연결선</mark>\
↳ `@Bindable`: 여러 <mark>속성을 연결할 수 있게 해주는 도구</mark>