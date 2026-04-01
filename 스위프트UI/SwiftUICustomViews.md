# 01 SwiftUI가 제공하는 방식
➜ SwiftUI는 사용자 인터페이스 디자인을 선언형(declarative) 방식으로 제공

전통적인명령형 방식에선 뷰를 생성, 배치, 설정하는 책임과 상태 변화에 따라 뷰를 지속적으로 업데이트하는 책임까지 컨트롤러 코드가 부담했다

반면, 선언형 방식에서는 UI의 원하는 레이아웃을 반영하는 뷰 계층을 선언함으로 사용자 인터페이스의 간단한 설명을 만듦 \
그 이후에는 SwiftUI가 사용자 입력이나 상태 변화 같은 이벤트에 반응하여 뷰를 그리거나 업데이트하는 과정을 관리



인터페이스 내에서 뷰를 정의하고 구성하기 위한 도구 제공
SwiftUI가 제공하는 내장 뷰들과 이미 정의된 다른 컴포지트 뷰들을 조합하여 커스텀 뷰를 만들 수 있음
뷰는 수정자를 통해 설정, 데이터 모델과 연결할 수 있음 ⋯ 이후 이러한 커스텀 뷰를 앱의 뷰 계층 안에 배치 ⭕

# 02 View 프로토콜을 따르기
커스텀 뷰 타입을 선언하려면? \
➜ `View` 프로토콜을 따르는 구조체를 정의해야함

- view 프로토콜은 특정 기능의 설계도를 제공 (화면에 그리는 요소의 동작을 정의)
- 프로토콜을 따르기 위해 필요한 요구사항을 충족하고 나면, 커스텀 뷰를 뷰 계층에 삽입해 앱의 사용자 인터페이스를 만들 수 있음 !!

# 03 boby 선언하기
View 프로토콜의 주요 요구사항 → body라는 계산 프로퍼티를 정의하는 것
```swift
struct MyView : View {
    var body : some View {
    }
}
```
뷰를 업데이트할 필요가 있을 때마다 이 프로퍼티의 값을 읽음 \
이러한 업데이트는 사용자 입력이나 시스템이벤트에 반응하여 반복적으로 일어날 수 있음 \
이때 반환된 값은 SwiftUI가 화면에 그리는 요소 !!

View의 프로토콜의 부가적인 요구사항은 boby 프로퍼티에 대한 연관 타입을 지정해야 한다는 것 \
하지만 이 타입을 명시적으로 선언할 필요는 ❌

대신 `some View`라는 불투명 타입을 사용하여 body의 반환 값이 View 프로토콜을 따른다는 사실만 명시 !!
# 04 뷰 구성하기
## 4-1 뷰 콘텐츠 구성하기
`body` 프로퍼티에 콘텐츠를 추가하여 뷰의 외형을 설명 \
SwiftUI에서 제공하는 내장 뷰는 물론, 다른 곳에서 정의한 커스텀뷰도 사용 ⭕ \
ex.) 내장 Text 뷰를 사용하여 "Hello, World"라는 문자열 표시 가능
```swift
struct MyView : View {
    var body : some View {
        Text ("Hello, World!")
    }
}
```
- SwiftUI는 Text, Toggle, ProgressView와 같은 특정 콘텐츠를 위한 뷰 외에도, 다른 뷰들을 정렬하는데 사용할 수 있는 내장 뷰들을 제공함 \
ex.) VStack을 사용하여 두 개의 Text 뷰를 수직으로 쌓을 수 있음
```swift
struct MyView : View {
    var body : some View {
        VStack {
            Text ("Hello Strawberry Latte")
            Text ("nice to meet youuuu")
        }
    }
}
```
- 여러 자식 뷰를 입력받는 뷰는 일반적으로 ViewBuilder 속성으로 표시된 클로저를 통해 구성 \
(이 덕분에 호출하는 쪽에서는 특별한 문법 없이 여러 줄로 된 클로저를 사용할 수 있음)
## 4-1 뷰를 수정자로 구기하기
`body`에 정의된 뷰를 설정하려면 뷰 수정자를 적용하면 됨 \
수정자는 뷰에 호출되는 메서드일 뿐이며 호출 시 원래 뷰 대신 사용할 수 있는 새로운 뷰를 반환
```swift
struct MyView : View {
    var body : some View {
        VStack {
            Text ("Hello blueberry Latte")
                .font(.title)
            Text("Hi Strawberry Latte")
        }
    }
}
```

# 05 데이터 관리하기
뷰에 값을 전달하려면 속성(프로퍼티)을 추가하면 됨
```swift
struct MyView : View {
    let helloFont : Font

    var body : some View {
        VStack {
            Text ("Heelo, World!")
                .font (helloFont)
            Text ("Glad to meet you")
        }
    }
}
```
- 입력값이 변경되면 해당 변경사항을 감지하고 사용자 인터페이스의 영향을 받는 부분만 다시 그림
- 언제든지 뷰를 재초기화할 수 있으므로 뷰의 초기화 코드에서 큰 작업을 수행하는 것은 지양해야 함

# 06 뷰 계층에 추가하기
```swift
struct ContentView : View {
    var body : some View {
        MyView (hello.Font : .title)
    }
}
```
- 뷰를 정의한 후에는 내장 뷰와 마찬가지로 다른 뷰 안에 포함 ⭕ \
뷰를 어디에 나타내고 싶은지에 따라 해당 위치에 선언하면 됨
- 또는 새로운 장면의 루트 뷰로 사용할 수도 있음 \
ex.) macOS 환경에서 설정 창을 위한 Settings scene이나 watchOS에서 알림 화면을 위한 WKNotificationScene 등에서 사용


---
---
---



## Swift view custom 선언
**명령형** : 중간 과정이 있음 \
(1~100까지 다 명령해줘야함, Swift kit)
**선언형** : 중간 과정 없이 간단함 (SwiftUI)

### SwiftIU (스유)
#### 장점
1. 선언형
2. 코드가 짧다 \
⬇️ ex.) Hello를 출력하기 위해 필요한 코드
```swift
Text ("Hello")
```
3. 개발속도가 빠르다

#### 단점
1. 디테일이 부족
    1. 위치 선언 ❌ \
    Vsatck은 위치가 안적혀 있음
    ```swift
    Text ("UwU")
    -> 위치 선언이 안 되어있음
    ```swift
    label. frame = CG Rect (x:100, y:200)
    swiftUI kit은 보이는 것처럼 위치가 선언 되어있음
    ```
2. 스유로는 구현이 안 되는게 아직까지 많음 \
ex.) kit에서는 애니메이션을 구현하는게 따로 있는데 스유에선 그런게 없음

---
---

':' -> 등호 표시
```swift
struct Myview : view {
// == view랑 Myview랑 같냐 아니냐 따지는 구조체 (이 코드가 정말 맞냐)
// 여기안에 있는 코드는 계속 '읽는것'인데 계산 코드를 넣으면 로직으로 계산을 해버리니까 넣으면 안됨
}
```
class로는 선언 못함 \
그림을 그리다가 사람이 바뀌면 다시 그러야하는 것처럼 이것도 안에 있는 코드를 읽는데(그리는데) 

### 특징
1. 계산을 넣으면 안됨 \
상태변동이 일어나기 때문에 다시 그려야함 (=무한반복) \
만약 계산을 넣고 싶으면 다른 struct를 파야함

⭐️ `Text`하기 전에 `struct` 쓰고 써야함

# body 선언 !!
```swift
var boby : some view {  // view는 타입이 안 정해져있음 (불투명타입)
// view 마법사가 옴 (모든 코드에는 return이라는 반환값이 있어야하는데 위 코드에는 반환값이 없으므로 반환값을 다 적어야함)
/* ...some view(text, Btext ~)
이렇게 다 하나씩 적으면 너무 많아지기 때문에 불투명 타입 = view 마법사를 씀 */
}
```swift
some = any (**대충** 아무거나)
```swift
stract Myview : view{
    var body : some view {
        // 여기안에 텍스트 적어야함
    }
}
```
```swfit
some = any (**대충** 아무거나)
```swift
stract Myview : view{
    var body : some view {
        Text ("copy")
    }
}

⬇️⬇️ 빌드를 했는데 계속 읽지 않으면 copy가 capybara로 바꾸어도 copy로 출력

some = any (**대충** 아무거나)
```swift
stract Myview : view{
    var body : some view {
        Text ("capybara")
    }
}
```

stract `Myview : view {` -> 내가 보고있는 거랑 어떤 작업을 하고 있는거랑 같냐 안 같냐라는 의미 \
SwiftUI에서 업데이트가 일어나면 다시 그 값을 처음부터 끝까지 그림

```swift
let. label = UIlbel()
label.text = ("Hello")
```
⬇️
텍스트를 뷰로 침



#### VStack
```swift
strct Mtview : View {
    var body : some body {
        VStack {
            text ("Hello")
        }
    }
}
```
- 클로저 = 문법 설명 ❌


#### 수정자
```swift
strct Mtview : View {
    var body : some body {
        VStack {
            text ("Hello")
        }
    }
}
```

# 데이터 관리하기
```swift
strct Mtview : View {
    var body : some body {
        VStack {
            text ("Hello")
        }
    }
}
```
➡️ struct -> body -> UStack -> Hello -> \
stract -> body -> UStack -> HI \
뭘 수정하면 계속 계속 쌓아올림 = 비효율적 !! (Swift의 큰 단점)