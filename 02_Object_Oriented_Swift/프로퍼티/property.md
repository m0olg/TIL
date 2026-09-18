# 01 프로퍼티란?
 **→ 구조체, class 같은 곳에 구현을 해서 타입과 연관된 값을 표현할 때 사용** \
 구조체, 클래스, 열거형 내부에 구현 가능 (다만, 열거형 내부에는 연산 프로퍼티만 구현 ⭕)
# 02 프로퍼티의 종류
## 2-1 저장 프로퍼티
**→ 값을 저장하기 위해 선언하는 변수 혹은 상수**
- 클래스나 구조체에서만 활용 가능
- 저장 프로퍼티는 메모리상에 저장‼️
```swift
struct Quokka {
    var name: String = "퀔캌" // 값 저장
}

let q = Quokka()
print(q.name) // == 퀔캌
```

### Getter(Get) - 읽기 전용
- Getter는 저장 프로퍼티의 값을 임의로 연산, 반환함
    - var(변수)와 명칭, 그리고 선언된 자료 뒤에 {} 블럭으로 감싸주고
    - 내부에 get {}코드 블럭을 활용하여 연산 실행
```swift
struct Quokka {
    var name: String
    
    // 읽기 전용 (get만 있음)
    var description: String {
        return "이름은 \(name)인 쿼카"
    }
}

let q = Quokka(name: "퀔캌")

print(q.description) // 가능
// q.description = "바꾸기" 불가능
```



### Setter(Set) - 쓰기 전용
- Setter(set)는 저장 프로퍼티의 값을 간접적으로 설정, 반환함
    - 연산 프로퍼티 내부에 set(매개변수){} 코드블럭을 활용
    - 해당 연산 프로퍼티의 값을 ➜ (매개변수)로 넘겨주고, 내부에서 연산을 실행
```swift
struct Quokka {
    private(set) var age: Int = 0
    
    mutating func grow() {
        age += 1
    }
}

var q = Quokka()

print(q.age) // 읽기 가능
// q.age = 5 외부에서 수정 불가 ❌

q.grow() // 내부 메서드로만 변경 가능
```
## 2-2 연산 프로퍼티
→ 연산을 실행할 수 있는 프로퍼팅임 근데 저장은 안됨 ⭐⭐ \
연산 프로퍼티는 직접 값을 가질 순 없지만 다른 프로퍼티(=저장 프로퍼티)를 활용해 연산값을 부여 가능
```swift
struct Quokka {
    var name: String = "퀔캌"
    
    var description: String {
        return "이름은 \(name)인 쿼카 " // 계산해서 반환
    }
}

let q = Quokka()
print(q.description)
```

# 03 지역변수 및 전역변수
→ 저장 프로퍼티와 연산 프로퍼티의 기능은 함수, 메서드, 클로저, 타입 등의 외부에 위치한 지역/전역 변수에도 모두 사용 가능
```swift
var a: Int = 100
var b: Int = 200
var sum: Int {
    return a + b
}

print(sum) // 300
```