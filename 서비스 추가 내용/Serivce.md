# 01 Serivce란?
→ **서버와 통신하며 데이터를 요청하고 응답을 받아오는 역할**

즉, 앱과 서버 사이에서 통신을 담당하는 역할 <br><Br>

# 02 Service 레이어의 역할과 필요성
레이어가 없는 구조에선 뷰모델이나 뷰컨트롤러가 네트워크 요청, 데이터 파싱, 에러 처리까지 모두 담담하게 되어 \
코드 가독성을 떨어뜨리고 테스트를 어렵게 만듦

- **관심사 분리** : UI 레이어는 화면을 그리고 사용자 입력을 처리하는데만 집중, 데이터와 관련된 비즈니스 로직은 Serivce가 전담
- **재사용성 향상** : 하나의 `UesrService`를 로그인 화면, 마이페이지 화면 등 여러 곳에서 공유하여 사용 ⭕
- **유지보수 및 테스트 용이성** : 비즈니스 로직이 격리되어 있어 단위 테스트를 작성하기 쉬움 <br><br>


# 03 실제 구현 예시

###### ++ : 추가한 주석, + : 수정한 주석


### 1단계 : 서비스 준비 및 URL 검증
서버에 요청을 보내기 전에 안전한 통신을 위해 주소를 확인하고 준비하는 단계

```swift
// PostService

import Foundation

final class PostService {

    static let shared = PostService()
    // ++ 인스턴스를 하나만 생성하여 앱 전체에서 공유하는 "싱글톤 패턴" 적용

    private init() {}
    // ++ 외부에서 추가로 인스턴스를 생성하지 못하게 초기화 함수를 private로 제한

    private let baseURL = "https://jsonplaceholder.typicode.com/posts" 
   // BaseURL을 Let으로 선언한 이유는 서버 주소는 변경되지 않는 고정된 값이므로
   
    func fetchPosts() async throws -> [Post] {
    // + async : 비동기 방식으로 서버 데이터를 가져옴
    // + throws : 네트워크나 파싱 실패 시 에러를 발생 시킬 수 있음
    // → [Post] : 최종적으로 가공된 앱용 Domain Model 배열을 반환
 
        guard let url = URL(string: baseURL) else {
        // + 문자열 주소를 URL 타입으로 안전하게 변환 (올바르지 않으면 즉시 에러 반환하고 종료)
            throw NetworkError.invalidURL
        }
        // 만약 변환 못했을 경우(guard else) Thorw(오류가 발생할 수도 있다)을 통해
        // NetworkdError.invaliURL을 내보냄 (오류 메세지)
```
### 2단계 : 비동기 네트워크 요청 및 응답 수신
실제 서버에 노크를 하고 응답을 받아오는 핵심 단계
```swift
let (data, response) =
        // Data와 response을 튜플로 받음)
        // 밑에 data(from:url)로 보낸 GET요청에 대해 Dat와 response로 응답 받음)
        // ++ 좌변의 data, response는 우변의 결과물 두 개를 각각 담기 위한 바구니 2개 세트
        try await URLSession.shared.data(from: url)
        // Try: 에러가 발생할 수 있는 코드를 실행할 때 사용 (서버 통신 중 에러가 발생할 수 있기 때문에 사용)
        // await: 비동기 작업이 끝날 때까지 기다릴 때 사용 (서버 응답이 올 때까지 기다림)
        // URLseesion: Apple에서 미리 만들어둔 네트워크 요청 보내는 기능을 담음
        // URLSession.shared: : Apple이 미리 만들어둔 싱글톤 URLSession 객체를 사용
        // data(from:url): BaseURL로 서버에 'GET' 요청을 보냄
        // ++ 여기서 await를 만나면 일시정지 상태가 되고 다른 화면 UI 작업 등에게 CPU 제어권을 양보하므로 앱이 멈추지 않음
```

### 3단계 : 응답서 다운캐스팅 및 상태 코드 검증
서버가 보낸 응답이 **정상적인 성공 응답**인지 꼼꼼하게 검사
```swift
guard let httpResponse = response as? HTTPURLResponse else {
        // response as? HTTPURLResponse : 기존에 받은 BaseURL을 URLResponse타입으로 
        // 변환했는데 기존 타입에서 HTTPResponse타입으로 다운캐스팅 함 (statueCode)를 사용
        // 하기 위해서
        // ++ 부모 타입인 URLResponse엔 statusCode 속성이 없어서 자식 타입인 HTTPURLResponse로 안전하게 변환(as?)하는 과정 
            throw NetworkError.invalidResponse
        }
         // guard을 통해 다운캐스팅이 되면 넘어가고 아니면 NetworkError.invalidResponse
        // 값을 출력함
        // ++ 만약 웹(HTTP) 통신 응답이 아니거나 캐스팅이 실패하면 잘못된 응답으로 간주
        // let httpResponse: 변환하여 statusCode 사용이 가능한 BaseURL을 httpResponse로 선언

        guard 200...299 ~= httpResponse.statusCode else {
            throw NetworkError.serverError(httpResponse.statusCode)
        }
        // statusCode: Apple에서 미리 지정한 통신에 대한 상태(무슨 오류? 성공 등)을 가져옴
        // httpResponse의 statusCode가 200 ~ 299 범위라면
        // 정상적인 서버 응답으로 판단하여 아래 코드를 계속 실행
        
        // 하지만 200 ~ 299 범위가 아니라면
        // throw NetworkError.serverError(httpResponse.statusCode)
        // 통해 해당 상태 코드를 에러로 전달

        // ++ 404, 500 등이 나왔을 때, 에러 내부에 상태 코드를 실어서 호출한 곳(ViewModel)으로 던짐
```

### 4단계 : JSON 데이터 디코딩
서버가 보내준 컴퓨터용 포맷(JSON)을 swift가 읽을 수 있는 구조체로 번역
```swift
let dto = try JSONDecoder().decode(
            [PostDTO].self,
            from: data
        )
        
        // JSONDecoder(): JSON 데이터를 Swift 데이터로 변환하는 기능
        // decode(): JSON 데이터를 원하는 Swift 타입으로 변환
        // [PostDTO].self: PostDTO 형태의 배열(Array)로 변환하겠다는 의미
        // from: data: 서버로부터 받은 JSON 데이터를 사용
        // 즉 서버에서 받은 JSON 데이터를
        // [PostDTO] 형태의 Swift 데이터로 변환하는 코드
        // 여기서 받은 Data를 DTO(domain Model)로 흐름
        // ++ 디코딩도 에러가 발생할 수 있는(데이터 형식이 모델과 다를 때 등) 작업이므로 앞에 try가 붙음 !!
```

### 5단계 : 도메인 모델로 변환 및 반환
서버 맞춤형 데이터를 앱에서 쓰기 좋은 깔끔한 데이터로 가공하여 최종 출력
```swift
return dto.map {
            $0.toDomain()
        }
        //map: 배열의 데이터를 하나씩 변환하는 기능
        //$0: 현재 순서의 PostDTO 데이터
        //toDomain(): DTO 데이터를 Domain Model 형태로 변환
        // 즉 PostDTO 배열을
        // 앱에서 사용하기 쉬운 Domain Model 배열로 변환하여 반환하는 코드
        // ++ 서버 스펙 변경에 유연하게 대처할 수 있도록 완전히 앱 전용 순수 데이터 모델[Post]로 정제하여 탈출 시킴
    }
}
```

## 정리

### ① 1단계 : 서비스 준비 및 URL 검증
- 싱글톤 방식으로 선언된 `PostService`는 앱 전체에서 인스턴스를 단 하나만 만들고 공유해서 쓰도록 만들어짐 \
외부에서 새로 만들지 못하게 막아둔 것도 특징
- 변하지 않는 서버 주소(baseURL)를 안전하게 상수로 선언한 뒤 이 문자열 주소를 스유가 인식할 수 있는 **URL 타입으로 변환**
- 이때 주소가 올바르지 않으면 뒤쪽 코드를 실행하지 ❌, `invalidURL`이라는 오류 메시지를 내보내며 그 자링에서 빠르게 종료(guard)

### ② 2단계 : 비동기 네트워크 요청 및 응답 수신
- 변환된 URL 주소를 가지고 *애플이 미리 만들어둔 네트워크 기능인 `URLSession.shared`를 통해 서버에 **GET** 요청* 을 보냄
- 이 작업은 비동기(async)로 처리되므로 서버 응답이 올 때까지 스마트폰 화면을 멈추지 않고 기다림 (await)
- 서버가 응답하면 그 결과물로 **원시 데이터(`data`)와 응답서(`response`)를 동시에 바구니 세트(`tuple`)로 안전하게 받아옴**

### ③ 3단계 : 응답서 다운캐스팅 및 상태 코드 검증 (성공 여부 확인)
- 서버에서 무언가 받긴 했지만 이게 진짜 제대로 된 응답인지 확인해야함 \
우선 상태 코드를 확인하기 위해 일반 응답 타입을 HTTP 전용 응답 타입 (`HTTPURLResponse`)으로 변환(다운캐스팅)해줌 \
만약 실패하면 `invalidResponse` 오류를 던짐
- 변환이 잘 되었다면 애플이 지정한 표준 통신 상태코드 (statusCode)를 검사 \
코드가 `200 ~ 299`(정상 성공) 범위라면 안심하고 다음으로 넘어가고 만약 범위를 벗어난 오류 코드라면 그 상태 코드를 그대로 에러에 담아 위로 던짐

### ④ 4단계 : JSON 데이터 디코딩 (파싱)
- 서버가 보내준 데이터는 컴퓨터가 읽는 형태인 JSON 포맷의 날것 (date)임 \
이걸 스유 코드가 읽을 수 있게 번역해야함
- `JSONDecoder().decode` 기능을 사용해 서버 전송용 데이터 모델인 `[PostDTO]`(배열 형태)로 변환을 시도함 \
이 단계를 거치면 raw 데이터가 드디어 우리가 다룰 수 있는 Swift 데이터 객체(DTO) 흐름으로 바뀌게 됨

### ⑤ 5단계 : 도메인 모델로 변환 및 반환
- 번역된 `PostDTO` 데이터는 아직 서버의 입맛에 맞춰진 규격임 \
이걸 우리 앱 화면(UI)에서 쓰기 편한 형태로 가공해야함
- 배열 안의 데이터를 하나씩 꺼내어 변환해주는 `map` 함수를 돌려서 DTO 데이터를 앱 전용 모델인 Domain Model(`Post`) 형태로 한땀 한땀 변환 (toDomain()) 해줌
- 정제가 완전히 끝난 깔끔한 `[Post]` 배열 데이터가 최종적으로 반환 (`return`)되어 화면을 그리는 뷰 모델 측으로 전달됨 <br><br><br>

# 04 자주 쓰이는 Serivce 종류
| 서비스 종류 | 주요 역할 |
| :--- | :--- |
| **NetworkService**<br>(네트워크 서비스) | • 서버 API에서 JSON 데이터(피드, 상품 목록 등) 다운로드하기<br>• 사용자가 작성한 글 서버에 업로드하기 |
| **AuthService**<br>(인증 서비스) | • 애플 로그인, 카카오 로그인 처리하기<br>• 로그인 토큰을 안전하게 보관하고, 로그아웃 시 지우기 |
| **StorageService**<br>(저장소 서비스) | • 인터넷이 안 돼도 글을 볼 수 있게 `SwiftData`나 `CoreData`에 저장하기<br>• 다크모드 설정 같은 간단한 설정을 `UserDefaults`에 저장하기 |
| **AnalyticsService**<br>(통계 서비스) | • 사용자가 '구매하기' 버튼을 몇 번 눌렀는지 Firebase 등에 기록 보내기<br>• 앱이 어디서 튕겼는지(크래시 로그) 수집하기 |
| **NotificationService**<br>(알림 서비스) | • "새로운 댓글이 달렸어요!" 같은 푸시 알림 권한 요청하기<br>• 알림을 탭하고 들어왔을 때 특정 화면으로 바로 이동시켜주기 |
message.txt
12KB