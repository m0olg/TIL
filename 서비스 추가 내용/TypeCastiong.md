# 01 Type Casting이란?
→ 타입 캐스팅은 인스턴스의 **타입**을 확인하거나 해당 인스턴스를 슈퍼 클래스나 하위 클래스로 취급하는 방법 <br><br>

# 02 `is` : Checking Type
```swift
표현식 is Type
```
**타입을 체크하는 연산자**로 \
런타임 시점에 실제 체크가 이루어짐, 표현식이 Type과 동일하거나 표현식이 Type의 서브 클래스인 경우 *true* \
이 외의 경우엔 *false* \
(반환 값은 Bool 형)

```swift
class Human { }
class Teacher: Human { }

let teacher: Teacher = .init()
teacher is Teacher      // true
teacher is Human        // true
```
이렇게 Human 클래스를 Teacher이란 클래스가 "상속" 받을 경우,
teacher이란 인스턴스는 Human 클래스의 서브 클래스이기 때문에 이런 경우 Human으로 타입 체크를 해도 true가 된다는 것

```swift
class Human {
    let name: String
    init(name: String) {
        self.name = name
    }
}
class Teacher: Human { }
class Student: Human { }
 
 
let people: [Human] = [
    Teacher.init(name: "김선생"),
    Student.init(name: "박제자"),
    Student.init(name: "유제자")
]
```
Human이란 클래스가 있고 이 클래스를 상속 받는 서브 클래스
Teacher, Student가 각각 존재
그리고 people이란 배열에 Teacher 인스턴스 1개, Student 인스턴스 2개를 담음

타입에 민감한 swift에서 두 개의 타입 인스턴스를 어떻게 저장하나 싶겠지만 \
**업캐스팅**으로 가능 한 것

```swift

for human in people {
    if human is Teacher {
        print("나는야 선생님 : \(human.name)")
    } else if human is Student {
        print("나는야 제자  : \(human.name)")
    }
}
```
이렇게 타입 캐스팅을 통해 확인하면서 조건문 분기를 할 수 있음

```swfit
// 결과
나는야 선생님 : 김선생
나는야 제자 : 박제자
나는야 제자 : 유제자
```
<br>
<Br>

# 03 `as` : Type Casting
```swift
표현식 as (변환 할)Type
표현식 as? (변환 할)Type
표현식 as! (변환 할)Type
```

<br>


## 3-1 업캐스팅 (Upcasting)
→ 서브 클래스 인스턴스를 **슈퍼 클래스의 타입**으로 참조함 \
업 캐스팅은 항상 성공, `as` 연산자를 사용해서 할 수도 있음 (컴파일 시점에 캐스팅 가능 여부 결정)

###### (아까 본 이상한 예제를 가지고 설명하겠다)
```swift
class Human {
    let name: String
    init(name: String) {
        self.name = name
    }
}
class Teacher: Human { }
class Student: Human { }
 
 
let people: [Human] = [
    Teacher.init(name: "김선생"),
    Student.init(name: "박제자"),
    Student.init(name: "유제자")
]
```
**swift는 타입에 민감한 언어**이므로 people이란 배열엔 Human이란 타입의 인스턴스만 들어갈 수 있음 \
근데 Teacher, Student라는 타입의 인스턴스는 어떻게 들어간 것일까\
. \
. \
. \
이것을 가능하게 해주는게 바로 **업캐스팅**

Teacher, Student란 클래스는 분명 서로 다른 타입의 클래스지만 공통점이 있는데 바로 \
부모 클래스가 같다는 것 즉, **둘의 슈퍼 클래스가 Human으로 동일 하므로 이 둘을 Human이란 클래스로 업캐스팅 해서 묶어버린 것**


하지만 *human이 Teacher이란 서브 클래스를 Human이란 슈퍼클래스 타입으로 참조하는 **업캐스팅**을 한 것이기 땜문에 human의 접근 범위가 "Human" 멤버로 한정됨*

```swift
human.name          // Sodeul
human.subject       // Value of type 'Human' has no member 'subject'
```
- Human 클래스의 멤버인 name엔 접근할 수 있음
- 하지만 서브 클래스인 Teacher의 멤버인 subject엔 접근 ❌


> 이렇게 서브 클래스의 인스턴스를 슈퍼 클래스의 타입으로 참조하는 걸 **업캐스팅**이라고 함
> 업캐스팅은 **항상 성공**하기 때문에 as를 써서해도 되고 직접 타입을 명시해서 해도 됨

<br>
<br>

# 3-2 다운캐스팅 (Downcasting)
→ 슈퍼 클래스의 인스턴스를 **서브 클래스의 타입**으로 참조함, 업캐스팅 된 인스턴스를 다시 원래 서브 클래스 타입으로 참조할 때 사용하고 \
다운 캐스팅은 실패할 수도 있기에 `as?`, `as!` 연산자 사용 (런타임 시점에 캐스팅 가능 여부 결정)

(이번에도 아까 본 예제로 설명 !!)
```swift
// human은 실제 Teacher 인스턴스로 생성되었지만,
// 업캐스팅 때문에 Human 타입으로 참조되고 있는 상태

human.name         // Sodeul (부모 멤버는 접근 가능)
human.subject      // Value of type 'Human' has no member 'subject' (자식 멤버는 접근 ❌)
```
업캐스팅을 할 경우 슈퍼 클래스의 멤버만 접근 가능하단 것을 방금 봄 \
실제론 Teacher 인스턴스를 생성했지만 업캐스팅 때문에 Human 멤버에만 접근 가능하게 제한


그렇다면 제한된 Teacher라는 서브 클래스의 subject 멤버에 다시 접근하고 싶다면?????????
= 이때 사용하는 것이 **다운 캐스팅**

```swift
var teacher: Teacher = human as! Teacher
```
이렇게 `as`연산자를 사용해서 상위 타입인 Human으로 묶여있던 변수를 다시 하위 클래스인 Teacher 타입으로 변환해서 넣어주는 것 \
. \
. \
이제 teacher 변수는 원래 본인 타입ㅇ인 Teacher 클래스로 다운 캐스팅 되었으므로 부모 멤버뿐만 아니라 본인의 고유 멤버에서 정상 접근 ⭕ \
`teacher.subject` = 접근 가능 !!

---

하지만 문제점이 하나 있는데 \
*항상 성공하는 업캐스팅* 과는 달리 **다운 캐스팅은 실패할 가능성이 존재**


만약 알맹이는 Teacher인데 코드를 잘못 짜버려서 Student 타입으로 다운 캐스팅을 시도하면
```swift
var student: Student = human as! Student    // 런타임 에러 발생
```
이렇게 다운 캐스팅이 실패해서 앱이 꺼지는 것을 막으려고 `as` 뒤에 옵셔널 (`?`, `!`)을 붙여서 제공 <br><br>


### `as?` vs `as!`

|        | `as?` (Conditional Cast) | `as!` (Forced Cast) |
| :--- | :---: | :---: |
| **캐스팅 방식** | **조건부** 다운 캐스팅 | **강제** 다운 캐스팅 |
| **반환 타입** | **Optional** 타입 (성공 시 `Optional(T)`) | **Non-Optional** 타입 (성공 시 `T`) |
| **실패 시 결과** | nil을 리턴 (앱이 죽지 않음) | 런타임 에러 발생 |
| **안전성** | 매우 안전함 | 위험함 (실패 가능성이 없을 때만 사용) |
| **주요 활용** | `if let` 이나 `guard let` 바인딩과 함께 사용 | 확실하게 해당 타입이 맞다고 보장할 수 있을 때 사용 |

#### = 쉽게 한 줄 정리해서 말하면 `as?`는 실패하면 nil을 뱉는 안전한 연산자, `as!`는 실패하면 에러(크래시)를 뱉는 위험한 연산자


<br>
<br>




# 04 URLResponse 타입 대표 기능 (부모)
- url : 응답 URL
- mimeType : 데이터 타입 (json, image 등)
- expectedContenLength : 예상 데이터 크기
- textEncodingName : 문자열 인코딩 방식
- suggestedFilename : 서버가 추천하는 파일 이름 (다운로드 한 파일 저장할 때 유용 !!)

<br>


<br>


# 05 HTTPURLRespose 타입 대표 기능 (자식)
→ HTTPURLResponse 클래스는 URLResponse 클래스의 서브 클래스, \
URL request의 응답과 연계된 메타 데이터를 캡슐화하고 이 클래스를 사용해서 HTTP 헤더 필트와 응답 상태 코드에 접근 가능 ⭕

- statusCode : HTTP 상태 코드 (200, 404 등)
- allHeaderFields : 서버 헤더 정보
- value(forHTTPHeaderField:) : 특정 헤더 값 가져오기

```swift
// 1. 서버에서 응답(리스폰)을 받았는데, 타입이 부모인 URLResponse인 상태
URLSession.shared.dataTsak(with: request) { data, response, error in
    
    // 2. 부모 타입을 자식 타입(HTTPURLResponse)으로 다운 캐스팅 (여기 완전 중요 !! 🐔🐔🐔)
    // + 만약 HTTP 통신이 맞다면 캐스팅에 성공해서 'httpResponse'라는 새로운 변수에 담김
    guard let httpResponse = response as? HTTPURLResponse else {
        print("error | HTTP 통신 응답이 아닙니다")
        return      // 캐스팅 실패 시 더 이상 아래 코드 실행 하지 않고 안전하게 종료
    }
    
    // 3. 이제 다운 캐스팅에 성공했으니 자식만의 고유 기능인 'statusCode'를 마음껏 씀
    switch httpResponse.statusCode {
        
    case 200...299:
        print("로그인 성공 | 서버가 성공 코드를 보냈습니다.")
        
    case 400, 401:  // 클라이언트 잘못
        print("로그인 실패 | 아이디나 비밀번호가 틀렸습니다. (상태코드: \(httpResponse.statusCode))")
        
    case 500...599: // 서버 잘 못
        print("서버 에러| 서버 컴퓨터가 아파서 응답을 못 합니다ㅜㅡㅜ")
        
    default:
        print("기타 에러 발생 (상태코드: \(httpResponse.statusCode))")
    }
    
    // 4. 부모가 가진 원래 기능(url, mimeType)도 자식은 상속받았기 때문에 당연히 쓸 수 있음
    if let currentURL = httpResponse.url {
        // 안전하게 사용하기 위해 if let으로 옵셔널 바인딩 (.url이 옵셔널 타입이므로)
        print("현재 응답을 받은 주소: \(currentURL.absoluteString)")
    }
}.resume()  // 비동기 네트워크 데이터 태스크를 실제로 실행 시키는 지인짜 중요한 메서드
```

### ⭐⭐ 요약
- `URLResponse` (부모) : 그냥 일반적인 모든 통신 응답 (기능은 별로 없음)
- `HTTPURLResponse` (자식) : 웹과 앱에서 쓰는 HTTP 통신 전용 응답 (상태 코드, 헤더 등 핵심 기능 많음)
- 실무에선 서버가 잘 응답했는지 검사해야 하므로 `as?, HTTPURLResponse`로 타입을 구제화 (배웠던 다운 캐스팅을 사용)해서 `statusCode`를 꺼내 쓰는 것이 필수 !!!!!!!
