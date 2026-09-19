### Service
# 01 `Service`란?
서비스는 MVC 패턴에서 Controller가 너무 무거워지는 것을 방지하기 위해 사용하는 심부름꾼 같은 역할임

원래 MVC에선 Controller가 서버 통신 코드를 직접 다 가지는 경우도 있긴한데 \
그러면 코드가 너무 길어지고 복잡해짐 그래서 '서버 통신만 전담하는 반장'을 따로 뽑는게 __서비스 레이어__

# 02 왜 서비스가 필요할까?
- 재사용성 : 로그인 통신 코드를 한 곳에 만들어두면, 메인 화면에서도 설정 화면에서도 불러다 쓸 수 있음
- 가독성 : ViewController는 _화면을 어떻게 그릴지_ 에만 집중하고, _데이터를 어떻게 가져올지_ 는 서비스에게 맡겨버림
- 유지보수 : 서버 주소가 바뀌거나 통신 방식이 바뀌어도 서비스 코드만 고치면 됨

# 03 흐름
| 단계 | 주체 | 행동 |
| :--- | :--- | :---: |
| 1 | __ViewController__ | 서비스에게 함수 호출 (ex. "서비스 반장아 사용자 목록 좀 가져와줘 ㅠㅠ") |
| 2 | __Service__ | 서버 주소로 요청을 보내고 결과 (JSON)를 받아서 model 구조체로 변환 |
| 3 | __ViewController__ | 서비스가 준 데이터를 받아서 화면에 뿌려줌 |
# 04 예제
```swift
import Foundation

// 1) 'Model' 서버에서 받아올 데이터의 모양
struct User: Codable {
    let name: String // 서버 JSON의 'name' 키 값과 똑같은 이름의 변수 만들기
}

// 2) 'Service' 서버에서 데이터를 가져오는 역할만 함
class UserService {
    // 결과를 화면(VC)에 전달하기 위해 completion이라는 보고서를 씀
    func fetchName(completion: @escaping (String) -> Void) {
        // 1. 도착지 주소 설정
        let url = URL(string: "https://api.github.com/users/m0olg")!
        
        // 2. 인터넷 연결해서 데이터 가져오기 (비동기)
        URLSession.shared.dataTask(with: url) { data, _, _ in
            // 3. 데이터가 잘 왔고 바구니(User)에 잘 담겼는지 확인
            if let data = data,
               let user = try? JSONDecoder().decode(User.self, from: data) {
                // 성공했다면 화면(VC)에게 이름을 알려줌
                completion(user.name) // 그리고 이름을 밖으로 전달!
            }
        }.resume()
    }
}

// 3) 'ViewController' 서비스에게 시켜서 화면에 보여주는 역할
class MyViewController {
    // 심부름꾼(서비스)을 한 명 고용한다고 보면 됨
    let service = UserService()

    // 화면이 켜질 때 실행되는 함수
    func start() {
        service.fetchName { name in // 이름 가져와달라고 요청
            print("결과: \(name)")    // 화면에 출력
        }
    }
}
```