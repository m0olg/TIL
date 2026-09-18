# 01 싱글톤이란?
→ **하나의 객체를 생성**한 뒤, **앱 전체에서 공유**하여 사용하는 방식 <br><br>


# 02 일반 객체와 싱글톤의 차이점  


## 2-1 일반 객체 Class
```swift
clss Player {       // hp가 100인 플레이어 설계도를 만듦
    var hp = 100
}
```

## 2-2 일반 객체
```swift
let player1 = Player()  // 위 'Player' Class를 통해 hp가 100인 Player1를 생성
let player2 = Player()  // 위 'Player' Class를 통해 hp가 100인 Player2를 생성

player1.hp = 50         // Player1의 hp를 100에서 50으로 바꿈

print(player1.hp)       // ❗Player1의 현재 hp 출력
print(player2.hp)       // ❗Player2의 현재 hp 출력
```
➡️ 50 : Player1의 현재 hp 출력값 \
➡️ 100 : Player2의 현재 hp 출력값


- `Player1`과 `Player2`는 서로 다른 객체 (메모리상)
- 그러므로 `player1.hp`를 변경해도 `player2.hp`는 변경 ❌

---

## 2-3 싱글톤 Class
```swift
final class Player {    // hp가 100인 Player 설계도를 만듦
    static let shared = Player()
    var hp = 100
    private init()
}
```

## 2-4 일반객체
```swift
let player1 = Player.shared     // 싱글톤으로 생성된 Player 객체를 player1에 넣음
let player2 = Player.shared     // 싱글톤으로 생성된 Player 객체를 player2에 넣음
player1.hp = 50                 // player1을 통해 Player 객체의 hp를 50으로 변경
print(player1.hp)               // player1이 바라보는 Player 객체의 hp 출력
print(player2.hp)               // Player2도 Player 객체를 바라보므로 동일한 hp 출력
```

- `player1`과 `player2`는 서로 다른 객체가 아니라 같은 객체를 공유함 (메모리상 같은 주소)
- 즉, `player1.hp`를 변경하면 `player2.hp`도 같은 값으로 변경됨

# 03 싱글톤을 사용하는 이유
-> 앱 전체에서 하나의 데이터를 함께 사용하기 위해 싱글톤을 사용

# 04 싱글톤을 사용하는 상황
- 로그인 사용자 정보 관리
- 게임 플레이어 정보 관리
- 네트워크 (Service) 관리
- 앱 설정값 관리
- 음악 플레이어 관리

### 와 같은 앱 전체에서 같은 데이터를 공유해야 하는 상황에 사용 ⬇️⬇️

```swift
let screen = UIScreen.main                      // 기기 화면의 크기 및 해상도 정보..? 관리
let userDefault = UserDefaults.standard         // 간단한 데이터 저장 / 관리
let application = UIApplication.shared          // 앱의 상태 및 생명주기
let fileManager = FileManager.default           // 기기 내 파일 시스템 관리
let notification = NotificationCenter.default   //앱 내의 객체 간 메시지 / 이벤트 통지 관리
```
# 05 싱글톤의 장단점
## 장점
- 하나의 객체를 공유하기 때문에 메모리 사용 ⬇️
- 어디서든 **같은 데이터에 접근** 가능
- 데이터를 **쉽게 공유** 가능

## 단점
- 모든 곳에서 같은 객체를 사용하기 때문에 **값이 쉽게 변경**될 수 있음
- 객체 간 **의존성이 강해**질** 수 있음
- **테스틍와 유지보수가 어려워**질 수 있음 \
ex.)  player.shared.hp = 50 한 곳에서의 값을 바꾸면 앱 전체에 영향이 갈 수 있다 <br><br><br>

# 06 싱글톤 패턴 (Singleton Pattern)
→ 특정 용도로 **객체를 하나만 생성하여 공용으로 사용하고 싶을 때** 사용하는 디자인 유형

```swift
class UserInfo {
    var id : String?
    var Password : String?
    var name : String?
}
```
1. User의 정보를 저장하는 클래스를 만듦
2. A ViewController에선 id를, B ViewController에선 password를, C ViewController에선 name을 입력 받아 이를 **UserInfo라는 클래스에 저장**해야 함
```swift
// A ViewController
let userInfo = UserInfo()
userInfo.id = "Sodeul"
```

```swift
// B ViewController
let userInfo = UserInfo()
userInfo.password = "123"
```

```swift
// C ViewController
let userInfo = UserInfo()
userInfo.name = "Sodeul"
```
3. 이런식으로 A, B, C 뷰컨트롤러에서 각각 UserInfo 객체를 만들어서 저장
4. 그러면 각 Instance의 프로퍼티에만 저장됨 \
이러면 원하는 그림이 아님 **(원하는 건 한 Instance에 모든 정보가 저장 되어야 함)**

###### 이럼 방법이 하나 있긴한데 인스턴스가 참조 타입이므로 User Info 인스턴스를 한번 생성한 후 이 인스턴스를 A, B, C로 필요할 때마다 참조로 넘겨줄 수 있긴한데 그러면 코드도 지저분해지고 귀찮음 ㅠ.ㅠ

*이 클래스에 대한 Instance는 최초 생성될 때 딱 한번만 생성해서 전역에 두고, 그 이후로는 이 Instance만 접근 가능하게 하자* \
는게 **Singleton Pattern**임


++
이런 식으로 한 인스턴스에 어디 클래스에서든 접근 가능하게 하는 것

<br>
<br>
<br>

# 07 추가로 알면 좋은 것들
## ① 싱글톤 객체 이름은 항상 shared여야 하는가
```swift
class Singleton {
    static let shared = Singleton()
    private init() {}
}

Singleton.shared.저쩌구메서드로_접근()
```
➜ 결론부터 말하면 싱글톤의 객체 이름이 항상 shared일 필요는 ❌

shared는 네이밍 관례일 뿐, 문법적 강제성을 띄고 있지 않음\
.\
.\
.
> 하지만 싱글톤의 역할에 따라 때로 더 적합한 이름이 있을 수 있음  
- default : 가장 대표적인 설정, 기본으로 설정된 규칙을 의미할 때
- standard : 이름처럼 표준 규격으로 만들어진 인스턴스 의미를 강조하고 싶을 때
- main : 메인 스레드 (UI)와 관련된 인스턴스 혹은, 메인 역할을 담당하는 인스턴스 의미를 강조할 때
- current : 현재 활성 상태를 담고 있는 인스턴스 의미를 강조할 때

<br>

## ② 암묵적 의존성 (Hidden Dependency) 문제와 개선
필연적으로 싱글톤과 싱글톤을 채택하는 클래스 사이에 암묵적 의존성 (의존성 은닉) 이 강한 결합 (Tight Copling) 관계로 발생
- class 내부에서 싱글톤 인스턴스를 직접 참조하기 때문에 외부에서 명시적으로 어떤 의존성을 가지고 있는지 드러나지 않게 됨 \
일반적으로 이 상황을 암묵적 의존성이라고 부름
- 어떤 클래스가 MyService.shared를 사용해도 이것이 생성자나 메서드 상에서 MyService에 의존하고 있다는 사실을 알 수 없다는 의미

### 개선 방법
의존성 주입 (DI)과 프로토콜 기반 설계로 문제점 개선 가능