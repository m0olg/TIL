# 01 `.padding`이란?
→ 쉽게 말해서 뷰(텍스트, 이미지, 버튼) 주변에 여백을 주는 것 = 보이지 않는 공간을 추가 하는 것

# 02 방향 지정
### ① 위
```swift
.padding(.top, 값pt)
```
이렇게 하면 위쪽에 입력한 만큼의 공간이 생김
### ② 아래
```swift
.padding(.bottom)
```
### ③ 오른쪽
```swift
.padding(.trailing)
```
### ④ 왼쪽
```swift
.padding(.leading)
```

**나머지도 똑같이 방향 지정하고 , 다음에 값 입력하면 그만큼의 공간(여백)이 생김** \
(++ leading : 시작 방향 (왼쪽), trailing : 끝 방향 (오른쪽))

#### 그럼 괄호에 아무것도 안 넣으면 어떻게 될까?
그냥 `.padding()`이렇게 하면 뷰 주변에 여백을 만들어 줌 \
위, 아래, 오른쪽, 왼쪽 사방팔방 다 똑같이 여백을 줌 !!

# 03 예제
⬇️⬇️ `.padding` 없으면?
```swift
VStack() {
    Text("capy")
    Text("bara")
}
```
**== 출력 결과** \
카피바라 \
귀여움 담당


⬇️⬇️ `.padding`을 넣으면?
```swift
VStack() {
    Text("capy")
        .padding()
    Text("bara")
}
```
**== 출력 결과** \
(여백) 카피바라 \
귀여움 담당