1강 - 이름짓기 (naming)
# 01 Lower Camel Case #
→ 첫 단어는 소문자, 이후 단어들은 대문자로 시작
```swift
// 형태
firstWordSecondWord
```
```swift
// 예시
userName
getUserDate
totalPrice
.
.
.
```
### 특징 ###
- 첫 글자가 소문자
- 주로 변수명이나 함수명에 많이 사용됨

---

### Q. 언제 쓰일까 ###
### A. Swift에선 거의 기본값 (변수, 함수, 프로퍼티, 매개변수 등) ###
``` swift

let userName = "Lee" // let으로 상수 지정 userName으로 변수 이름, "Lee"로 문자열 값 입력

func getUserInfo() {
/* func는 함수 만들 떄 쓰는 키워드, ()는 입력값 없음을 뜻함 */
		print("info")
}
```
# 02 Upper Camel Case
→ 모든 단어의 첫 글자가 대문자!!
```swift
// 형태
FirstWordSeconWord
```
```swift
// 예시
UserName
GetUserDate
TotalPrice
.
.
.
```
### 특징
- 첫 글자도 대문자
- 클래스 / 타입에 주로 사용
