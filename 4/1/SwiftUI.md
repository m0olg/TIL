# 01 Swift View Custom 선언

| 구분 | 설명 | 언어 |
|:---:|:---:|:---:|
| 명령형 | 중간 과정이 있음 | SwiftUI kit |
| 선언형 | 중간 과정 없이 간단 | SwiftUI |

명령형은 중간 과정이 있어서 하나부터 열까지 하나하나 다 명령해줘야함

## 02 SwiftUI
## 2-1 장점
1. **선언형**
2. **코드가 짧다** \
\
⬇️ ex.) Hello를 출력하기 위해 필요한 코드
    ```swift
    Text ("Hello")
    ```
3. **개발 속도가 빠르다** \
코드가 짧다보니 개발 속도도 빠름 !!

## 2-2 단점
1. 디테일이 부족함 \
a. 위치 선언 ❌ \
    Vsatck은 위치가 안적혀 있음
    ```swift
    Text ("UwU")
    ```
    → 위치 선언이 안 되어있음

    .
    

    .
    ```swift
    label. frame = CG Rect (x:100, y:200)
   ```
    → swiftUI kit은 보이는 것처럼 위치가 선언 되어있음
2. **SwiftUI로는 구현이 안되는게 아직까지 많음** \
ex.) kit에서는 애니메이션을 구현하는게 따로 있는데 스유는 그런게 없음

---
---
 

```swift
struct Myview : view {
    // ↘️ view랑 Myview랑 같냐 아니냐 따지는 구조체 (이 코드가 정말 맞냐)
    // 여기안에 있는 코드는 계속 '읽는것'인데 계산 코드를 넣으면 로직으로 계산을 해버려서 넣으면 안됨
}
```
그리고 위 코드는 `class`로는 선언을 못한다 (Why? 그림을 그리는 방식 자체가 달라서) \
그림을 그리다가 사람이 바뀌면 다시 그려야하는 것처럼 이것도 안에 있는 코드를 읽는데 (그리는데) 사람이 바뀌면 그 종이에 그림을 고쳐서 그리는게 아닌 그냥 새 종이에 그림을 그림


++ ':' 콜론은 쉽게 말해서 등호 표시임

# 03 특징
1. 계산을 넣으면 안됨 \
상태변동이 일어나기 때문에 다시 그려야함 (=무한반복) \
만약 계산을 넣고 싶으면 다른 `struct`를 파야함

    #### ⭐️ `Text`하기 전에 `struct` 쓰고 써야함

# 04 body 선언
```swift
var boby : some view {  // view는 타입이 안 정해져있음 (불투명타입)
// view 마법사가 옴 (모든 코드에는 return이라는 반환값이 있어야하는데 위 코드에는 반환값이 없으므로 반환값을 다 적어야함)
/* ...some view(text, Btext ~)
이렇게 다 하나씩 적으면 너무 많아지기 때문에 불투명 타입 = view 마법사를 씀 */
}
```
`some = any` (**대충**, 아무거나라는 의미)
```swift
stract Myview : view {
    var body : some view {
        // 여기안에 텍스트 적어야함
    }
}
```

```swift
stract Myview : view {
    var body : some view {
        Text ("copy")
    }
}


⬇️⬇️ 빌드를 했는데 계속 읽지 않으면 copy가 capybara로 바꾸어도 copy로 출력


stract Myview : view {
    var body : some view {
        Text ("capybara")
    }
}
```

`stract Myview : view {` → 내가 보고있는 거랑 어떤 작업을 하고 있는거랑 같냐 안 같냐라는 의미 \
SwiftUI에서 업데이트가 일어나면 다시 그 값을 처음부터 끝까지 그림

```swift
let. label = UIlbel()
label.text = ("Hello")
```
⬇️
텍스트를 뷰로 침

# 05 VStack
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

# 06 수정자
```swift
Text("Hello")
    .font(.title)
    .foregroundColor(.red)
```
- __폰트나 색깔을 수정하는 것__ (새로운 뷰를 만들어 반환함)

# 07 데이터 관리하기
```swift
strct Mtview : View {
    var body : some body {
        VStack {
            text ("Hello")
        }
    }
}
```
➡️ struct → body → UStack → Hello → \
stract → body → UStack → HI \
뭘 수정하면 계속 계속 쌓아올림 = 비효율적 !! _(Swift의 큰 단점)_