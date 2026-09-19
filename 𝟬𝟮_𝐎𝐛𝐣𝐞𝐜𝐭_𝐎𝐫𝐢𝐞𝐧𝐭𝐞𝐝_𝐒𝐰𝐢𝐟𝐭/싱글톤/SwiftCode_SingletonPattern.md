## 코드 분석
# 01 `static let shared`
```swift
static let shared = SnackStore()
```
`shared`는 공용 간식 창고를 하나만 생성하고 보관함 \
치이, 하치, 우사기가 접근해도 모두 같은 창고를 사용

> 실제 상황에선 다음처럼 공용 관리자 객체에 접근 ⬇️⬇️
```swift
NetworkMaanager.shared
```
`static`은 타입 자체에 속하는 프로퍼티를 만듦 \
따라서 객체를 먼저 생성하지 않고 클래스 이름으로 접근 가능함 ( = `SnackStore.shared` 요러케)

# 02 `pravate init()`
```swift
private init() {
    snacks = ["밥", "토스트", "모몽가"]
}
```
생성자를 `private`으로 설정하면 외부에서 새로운 간식 창고를 만들 수 있음

```swift
let newStore = SnackStore() // 오류
```
네트워크 관리자 객체를 여러개 만들지 못하게 하고 반드시 `NetworkManager.shared`만 사용하도록 제한 가능
```swift
let networkMaanager = NetworkManager.shared
```

# 03 `final class`
```swift
// 싱글톤 관리자가 상속되어 다른 인스턴스를 만드는 상황을 방지 가능함
final class SnackStore
```
final은 해당 클래스를 상속할 수 없도록 함


# 04 `praivate(set)`
```swift
private(set) var snacks: [String]
```
외부에선 간식 목록을 직접 수정 할 수 없고 클래스 내부에서만 수정 가능


```swift
// 대신 메서드를 통해서 간식을 추가하거나 가져감
SnackStore.shared.takeSnack()
SnackStore.shared.addSnack("도토리")
```
> 실제 상황에선 로그인 상태를 외부에서 무분별하게 막 바꾸지 못하도록 메서드를 통해서만 변경하게 만들 수 있음

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
<details open>
<summary>코드로 보기 (스유와 iOS에선 이미 여러 싱글톤 객체를 제공)</summary>

## 7-1 `UserDefaults.standard`
앱의 간단한 설정이나 사용자 정보를 저장할 때 사용
```swift
UserDefaults.standard.set(true, forKey: "isLoggedIn")

let isLoggedIn = UserDefaults.standard.bool(forKey: "isLoggedIn")
```

## 7-2 `FileManager.default`
파일과 디렉터리를 관리할 때 사용
```swift
let fileManager = FileManager.default
```

## 7-3 `URLSession.shared`
네트워크 요청을 보낼 때 사용
```swift
let session = URLSession.shared
```

## 7-4 `NotificationCenter.default`
앱 내부에서 이벤트를 전달할 때 사용
```swift
// 로그인 완료 사실을 여러 화면에 알릴 때 사용 가능
NotificationCenter.default.post(
    name: Notification.Name("didLogin"),
    object: nil
)
```

</details>

# 08 싱글톤의 문제점
개념에서 말했듯이 결합도가 높아질 수 있음 (직접 참조하면 클래스간의 결합도 증가)
```swift
final class LoginViewModel {
    func logout() {
        SessionManager.shared.isLoggedIn = false
    }
}
// LoginViewModel이 세션 관리자에 직접 의존하므로 테스트에서 다른 세션 관리자를 사용하기 어려움
```
또한 앱의 여러 위치에서 값을 변경하면 상태를 추척하기 어려워짐
```swift
SessionManager.shared.isLoggedIn = false
// 로그아웃 코드가 여러 화면에 흩어져 있으면 로그인 상태가 변경된 위치를 찾기 어려울수도 잇음
```

# 09 의존성 주입으로 개선
↳ <sup><sup>싱글톤을 직접 사용하는 대신 필요한 객체를 외부에서 전달 받을 수 있음

```swift
protocol NetworkService {
    func request()
}
```
```swift
final class LoginViewModel {
    private let networkService: NetworkService

    init(networkService: NetworkService) {
        self.networkService = networkService
    }

    func login() {
        networkService.request()
    }
}
```
실제 앱에선 싱글톤 네트워크 관리자를 전달할 수 있음
```swift
let viewModel = LoginViewModel(
    networkService: NetworkManager.shared
)
```
그리고 텍스트용 객체를 만들 수도 잇음
```swift
final class MockNetworkService: NetworkService {
    func request() {
        print("테스트용 네트워크 요청")
    }
}
```
```swift
let testViewModel = LoginViewModel(
    networkService: MockNetworkService()
)
```
이렇게 하면 실제 서버에 요청하지 않고도 로그인 로직을 테스트 가능함

# 10 싱글톤을 사용하면 왜 좋을까???????
다음과 같이 앱 전체에서 하나만 존재하는 것이 자연스러운 객체에 사용 가능 <sup><sup><sup>이것도 개념에서 쓴거랑 비슷한 개념이라........ 그냥 훑어보기
```swift
final class AppSettings {
    static let shared = AppSettings()

    private init() {}

    var isDarkModeEnabled = false
}
```
> **🍀 사용법** : `AppSettings.shared.isDarkModeEnabled = true`
여러 화면에서 다크 모드 설정을 공유해야 하는 경우 등에 사용하면 귯


다만 네트워크 관리자나 DB 관리자처럼 테스트에서 교체해야하는 객체라면 의존성 주입을 함께 고려하는 게 좋음

# 11 핵심 코드
```swfit
final class Singleton {
    static let shared = Singleton()

    private init() {}
}
```

> **사용법** : `Singleton.shared`
> 실제 상황에서는 `NetworkManager.shared`, `SessionManager.shared`처럼 앱 전체에서 공유하는 관리자 객체에 적용할 수 있음

# 12 정리
* 스유에선 싱글톤을 다음과 같이 구현
    1. `static let`으로 하나의 인스턴스를 만듦
    2. `private init()`으로 외부 생성을 막음
    3. `final`로 상속을 제한
    4. `hared`를 통해 어디서든 접근
<sup><sup>먼작귀 예제에선 치이, 하치, 우사기가 하나의 공용 창고를 사용하는 것으로 표현햇음


* 실제 개발에선 다음과 같은 객체에 사용이 가능함
    1. `네트워크 관리자`
    2. `사용자 세션 관리자`
    3. `앱 설정 관리자`
    4. `파일 관리자`
    5. `UserDefaults`
    6. `FileManager`
    7. `URLSession`
싱글톤은 공용 간식 창고처럼 반드시 하나만 존재해야하는 객체에 적합함


### ✨ 하지만 모든 객체를 싱글톤으로 만들면 전역 상태와 높은 결합도가 발생할 수 있으므로 꼭 필요한 경우에만 사용하기 !!


