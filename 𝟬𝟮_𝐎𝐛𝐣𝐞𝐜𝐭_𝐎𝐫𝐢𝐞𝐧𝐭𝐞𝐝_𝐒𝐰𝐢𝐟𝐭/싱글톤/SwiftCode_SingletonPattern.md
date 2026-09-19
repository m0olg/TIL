## 코드 분석
# 01 `static let shared`
# 02 `pravate init()`
# 03 `final class`
# 04 `praivate(set)`
# 05 예시로 알아보기
```swift
// 화면에서 사용자가 상품을 장바구니에 추가하는 것과 비슷함
// 여러 화면에서 같은 장바구니 객체를 공유 가능 !!
let snackStore = SnackStore.shared

if let snack = snackStore.takeSnack() {
    print ("돼지우사기가 \(snack)을 가져가버렸어")
}
```
<sub><sub>하치와레도 같은 간식창고를 사용

```swift
if let snack = SnackStore.shared.takeSnack() {
    print("하치와레가 \(snack)을(를) 가져갓다")
}
```

<sub><sub>이제 여기서 치이가 간식을 추가

```swift
// 여러 화면에서 동일한 장바구니나 앱 설정 객체의 상태를 수정하는 경우
SnackStore.sharedd.addSnack("곰돌이젤리")
```



# 06 동일한 인스턴스인지 확인
```swift
let chiikawaStore = SnackStore.shared
let hachiwareStore = SnackStore.shared

print(chiikawaStore === hachiwareStore) // true
```
치이카와가 접근한 창고가 하치가 접근한 창공와 같은 객체이므로 `true`

# 07 Swift에서 제공하는 싱글톤 예시
<details>
<summary>코드로 보기 (스유와 iOS에선 이미 여러 싱글톤 객체를 제공)</summary>

## 7-1 `UserDefaults.standard`
앱의 간단한 설정이나 사용자 정보를 저장할 때 사용
```swift
UserDefaults.standard.set(true, forKey: "isLoggedIn")

let isLoggedIn = UserDefaults.standard.bool(forKey: "isLoggedIn")
```

## 7-2 `FileManager.default`