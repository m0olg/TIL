## 서비스
# 01 `Service`란?
→ 서버 통신, 데이터 저장, 인증처럼 특정 기능을 담당하는 객체

> ex) \
> <u>화면에서 직접 네트워크 요청을 보내는 대신 서비스가 서버와 통신하도록 역할을 분리 가능</u>함

<sup>여기서 또 먼작귀로 비유 해보자면 치이가 잡초를 뽑아서 보수를 받기 위해 직접 갑옷씨한테 연락(가는게)아니라 심부름을 담당하는 먼작귀족에게 부탁하는 상황
![alt text](서비스그림1.png)

# 02 `Service`를 사용하는 이유
화면에서 네트워크 요청까지 직접 처리하면 하나의 클래스가 너무 많은 일을 담당하게 됨
```swift
final class LoginViewController {
    func login() {
        // 아이디 확인
        // 비밀번호 확인
        // URL 생성
        // 서버 요청
        // 응답
        // 화면 업뎃 등등...
    }
}
```
**이렇게 되면 뭐가 문제냐**❓ <sup>코드가 복잡해지고 테스트하기 어려움 ㅠㅅㅠ</sup>

✨ 여기서 서비스를 사용하면 역할을 분리할 수 있음 !!
>`ViewController` : 사용자 입력과 화면 표시
> `ViewModel` : 화면에 필요한 상태 관리
> `Service` : 서버 요청과 응답 처리

# 03 기본 구조
```swift
// 마이페이지에 필요한 사용자 정보를 서버에서 가져오는 기능
final class UserService {
    func fetchUser() {
        // 사용자 정보를 가져오는 로직? 코드?
    }
}
```

> `ViewModel`은 `Service`를 사용
```swift
final class UserViewModel {
    private let userService = UserService()

    func loadUser() {
        userService.fetchUser()
    }
}
```
뷰모델이 서버 통신의 세부적인 내용까지 알 필요 없이 `userService.fetchUser()`만 호출하면 됨

# 04 Request와 Response (요청과 응답)
이 두개는 서비스에서 자주 사용되는 개념임
> 클라이언트 ━━ Request(요청) ➡ 서버 \
> 클라이언트 ⬅ Response(응답) ━━ 서버
* `Request` : 클라이언트가 서버에 보내는 요청
* `Response` : 서버가 클라이언트에 보내는 응답
<sup>먼작귀로 비유 = 우사기가 간식 목록 좀 알려조바라고 부탁하는게 요청이고 애들이 간식 목록을 보내주는 게 응답임

```swift
let request = URLRequest()

URLSession.shared.dataTask(with: request) { data, response, error in
    // data : 서버에서 받은 실제 데이터
    // response : 서버의 응답 정보
    // error : 요청 실패 시 오류
}.resume()
```

# 05 실제 네트워크 서비스 구현
```swift
import Foundation

// 서버와 통신하는 Service
final class UserService {

    // 실제 API 서버의 기본 주소
    private let baseURL = "https://sadsadTT.com"

    // 서버에서 사용자 정보를 가져오는 메서드
    // ===============================
    // completion:
    // → 성공하면 User 객체 전달
    // → 실패하면 Error 전달
    func fetchUser(
        completion: @escaping (Result<User, Error>) -> Void
    ) {
        // 요청을 보낼 전체 URL 생성
        guard let url = URL(string: "\(baseURL)/user") else {
            // URL 생성에 실패하면 메서드 종료
            return
        }

        // 서버에 보낼 요청 객체 생성
        let request = URLRequest(url: url)

        // 서버에 네트워크 요청
        URLSession.shared.dataTask(with: request) {
            data,
            response,
            error in

            // 네트워크 요청 자체가 실패한 경우
            if let error {
                completion(.failure(error))
                return
            }

            // 서버에서 받은 데이터가 없는 경우
            guard let data else {
                return
            }

            do {
                // JSON 데이터를 User 타입으로 변환
                let user = try JSONDecoder().decode(
                    User.self,
                    from: data
                )

                // 사용자 정보를 성공 결과로 전달
                completion(.success(user))

            } catch {
                // JSON 변환에 실패한 경우
                completion(.failure(error))
            }

        }
        // dataTask는 resume()을 호출해야 실제 요청이 시작됨
        .resume()
    }
}
```
사용자 모델은 이런식으로 정의 가능
```swift
struct User: Decodable {
    let id: Int
    let name: String
}
```

# 06 코드 흐름
### ① 일단 그림으로 보기

<img src="서비스그림2.png" width="350">

### ② 실행 순서
1. `ViewModel`이 `UserService`에 사용자 정보를 요청
2. `Service`가 `URLRequest`를 생성
3. `URLSession`이 서버에 `Request`를 보냄
4. 서버가 `Response`를 반환
5. `Service`가 응답 데이터를 `User` 객체로 변환
6. **성공** 또는 **실패** 결과를 `ViewModel`에 전달

# 07 `Result`를 사용하는 이유
네트워크 요청은 성공하거나 실패할 수 있음
`Result<User, Error>` 위 타입은 다음 두 가지 결과를 표현함
```swift
.success(User)
.failure(Error)
```

> 사용 예시는 다음과 같음 ⬇️⬇️
```swift
// 서버 요청이 성공하면 사용자 정보를 화면에 표시하고 실패하면 에러 메시지를 보여줌
let userService = UserService()

userService.fetchUser { result in
    switch result {
    case .success(let user):
        print("사용자 이름: \(user.name)")

    case .failure(let error):
        print("사용자 정보 요청 실패: \(error)")
    }
}
```

# 08 `Service`와 `ViewModel` 연결
```swift
final class UserViewModel {
    private let userService: UserService

    init(userService: UserService) {
        self.userService = userService
    }

    func loadUser() {
        userService.fetchUser { result in
            switch result {
            case .success(let user):
                print("사용자 정보 로딩 성공: \(user.name)")

            case .failure(let error):
                print("로딩 실패: \(error)")
            }
        }
    }
}
```

사용할 땐 서비스를 주입
```swift
let userService = UserService()
let viewModel = UserViewModel(userService: userService)

viewModel.loadUser()
```
* 이렇게 하면 `ViewModel`이 `Service`를 직접 생성하지 않고 외부에서 전달 받음

# 09 프로토콜을 사용한 설계
`Protocol`은 서비스가 어떤 기능을 가지고 있어야 하는지 미리 정해두는 **규칙**
```swift
protocol UserServiceProtocol {
    func fetchUser(
        completion: @escaping (Result<User, Error>) -> Void
    )
}
```
이제 `UserService`가 이 규칙을 따르도록 함
```swift
final class UserService: UserServiceProtocol {
    func fetchUser(
        completion: @escaping (Result<User, Error>) -> Void
    ) {
        // 여긴 요청 코드
    }
}
```
> 이렇게 하면 UserService가 fetchUser 기능을 반드시 가지고 있어야함 ⭐

#### 테스트할 땐 가짜 서비스를 사용 가능
```swift
final class MockUserService: UserServiceProtocol {
    func fetchUser(
        completion: @escaping (Result<User, Error>) -> Void
    ) {
        let user = User(id: 1, name: "테스트 사용자")
        completion(.success(user))
    }
}
```
➜ 즉, 실제 서버에 요청하지 않고도 테스트 가능함
```swift
let service = MockUserService()

service.fetchUser { result in
    print(result)
}
```
> `Protocol`을 사용하면 실제 서버용 Service와 테스트용 Service를 쉽게 바꿔 사용할 수 잇음 !!!!!!!

# 10 `Service`와 `Singleton`
서비스를 싱글톤으로 만들면 앱 어디서든 같은 객체에 접근이 가능함
```swift
final class NetworkService {
    static let shared = NetworkService()

    private init() {}

    func request() {
        print("네트워크 요청")
    }
}
```

> 사용법 : `NetworkService.shared.request()` 요런식~
<sup>먼작귀로 비유하자면 치, 하, 우사기가 하나의 공용 통신기를 함께 사용하는 상황임


#### 하지만‼️<sup>서비스를 항상 싱글톤으로 만들 필욘 없음</sup>

싱글톤은 편리하지만 다음과 같은 **단점**이 있음 ([싱글톤 개념](https://github.com/m0olg/iOS_TIL/blob/main/%F0%9D%9F%AC%F0%9D%9F%AE_%F0%9D%90%8E%F0%9D%90%9B%F0%9D%90%A3%F0%9D%90%9E%F0%9D%90%9C%F0%9D%90%AD_%F0%9D%90%8E%F0%9D%90%AB%F0%9D%90%A2%F0%9D%90%9E%F0%9D%90%A7%F0%9D%90%AD%F0%9D%90%9E%F0%9D%90%9D_%F0%9D%90%92%F0%9D%90%B0%F0%9D%90%A2%F0%9D%90%9F%F0%9D%90%AD/%EC%8B%B1%EA%B8%80%ED%86%A4/singletonPattern.md) 참고하기)
* 전역에서 접근할 수 있어 값이 어디서 바뀌었는지 추적하기 어려움
* 테스트할 때 다른 Service로 교체하기 어려움
* 특정 Service에 코드가 강하게 의존할 수 있음

그래서 테스트가 중요하거나 서비스를 교체해야 한다면 싱글톤보다 **의존성 주입**을 사용하는 게 조음
```swift
final class UserViewModel {
    private let userService: UserServiceProtocol

    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
}
```
요러케 실제 서비스를 넣을수도 있고
```swift
let viewModel = UserViewModel(
    userService: UserService()
)
```
테스트용 서비스를 넣을 수도 있음
```swift
let viewModel = UserViewModel(
    userService: MockUserService()
)
```
# 11 정리
`Service`는 <mark>특정 기능을 담당하는 객체</mark> \
특히 서버 통신을 `Service`로 분리하면 **화면 코드가 간결**해지고 각 객체의 **역할이 명확**해짐

#### 🍎 핵심 역할
* `Request` 생성
* 서버 통신
* `Response` 처리
* JSON 디코딩
* 성공과 실패 결과 전달

#### 🐰 핵심 흐름
```
ViewController
→ ViewModel
→ Service
→ Server
→ Service
→ ViewModel
→ ViewController
```

#### 🍀 기억할 점
* `Request`는 서버에 보내는 **요청**
* `Response`는 서버에서 받는 **응답**
* `Service`는 네트워크나 데이터 처리 로직을 담당
* `ViewModel`은 `Service`를 호출하고 화면에 필요한 상태를 관리
* `Protocol`과 **의존성 주입**을 사용하면 테스트하기 쉬워짐
* `Service`를 항상 `Singleton`으로 만들 필요는 없음
