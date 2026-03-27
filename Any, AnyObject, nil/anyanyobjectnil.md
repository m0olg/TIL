# 01 Any

**Swift의 모든 타입을 사용할 수 있는 타입**으로 변수 또는 상수의 데이터 타입이 Any로 선언 되었다면 어떤 종류의 데이터 타입이든 상관 없이 할당 가능!!
<details>
  <summary>ex</summary>

- 구조체 struct
- 열거형 enum
- 클래스 class
- 함수
- 튜플
- 옵셔널 값까지도

   **즉, 정말 무슨 타입이든 담을 수 있는 상자**임
</details>

```swift
var something: Any   // Any : 모든 타입의 값을 담을 수 있는 변수

something = "치이카와"
// String(문자열) 타입 저장

something = "치이카와는 닭발을 싫어해"
// 또 다른 String 값으로 덮어쓰기
// → Any는 타입 제한이 없어서 계속 다른 타입 넣어도 됨

something = false
// Bool(참/거짓) 타입 저장

something = ["닭발", "치이카와", "발로란트"]
// Array(배열) 타입 저장
// → 여러 개의 문자열을 하나로 묶은 리스트

something = (menu: "닭발", mood: "행복")
// → 서로 다른 의미의 값을 묶어서 저장

something = {
    print("치이카와는닭발을먹었다")
}
```


# 02 AnyObject
`Any`보단 조금 한정된 의미로 **클래스의 인스턴스만 할당** 가능
```swift
var something: Any

something = "치이카와"
something = "치이카와는 닭발을 안 좋아해"
something = false
something = ["닭발", "치이카와", "발로란트"]
something = (menu: "닭발", mood: "행복")
something = {
print("치이카와는닭발을먹었다")
}
class Food {
    var name = "닭발"
}

class Character {
    var name = "치이카와"
}

var box: AnyObject   // 클래스 전용 상자

box = Food()        // 가능 (클래스라서)
box = Character()   // 가능 (클래스라서)
```

# 03 nil
값이 없다는 뜻 (비어있음)
```swift
var name : String? = nil
```
→ `name`은 문자열일 수도 있고 **아무 값도 없을수도 있음(nil)**

### 주의 ‼️
```swift
var a : String = nil // 오류 발생
```
→ `String`은 무조건 값 있어야 하는 타입임

## ++ 추가 설명
### TIP
`AnyObject`가 먼저 보이면 참조 타입만 허용하려는 의도구나라고 보면 됨  \
`Any`는 유연하지만 타입 안정성을 희생, `AnyObject`는 주로 클래스 전용 제약에 중요 \
`nil`은 옵셔널만 가능, `?` = `nil` 허용


### 정리
- `Any`는 무엇이든 담는 범용 타입, `AnyObject`는 클래스 인스턴스만 담는 범용 타입
- `nil`은 값이 없다는 의미
