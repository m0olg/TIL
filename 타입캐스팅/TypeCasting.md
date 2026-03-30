# 01 타입캐스팅
인스턴스의 타입을 확인하건나, 해당 인스턴스를 슈퍼 클래스나 하위 클래스로 취급하는 방법
(`is`, `as` 연산자로 구현)

기본 클래스 ⬇️
```swift
class Animal { }

class Cat: Animal {
    func meowong() {
        print("미용미용")
    }
}
```
# 02 업 캐스팅
`as`를 사용하여 부모클래스의 인스턴스로 사용할 수 있도록 컴파일러에게 타입정보를 전환 \
`Any`, `AnyObject`로도 타입정보를 변환 가능, 암시적으로 처리되므로 꼭 필요한 경우가 아니라면 생략해도 상관 ❌❌
```swift
let cat = Cat()
let animal: Animal = cat as Animal
```
자식 -> 부모 (항상 성공, 안전)
# 03 다운 캐스팅
`as?` 또는 `as!`를 사용하여 자식 클래스의인스턴스로 사용할 수 있도록 컴파일러에게 인스턴스의 타입정보를 전환
```swift
let animal: Animal = Cat()

if let cat = animal as? Cat {
    cat.meowong()
}
```
실패하면 `nil` -> 안전하게 사용
## 3-1 조건부 다운 캐스팅
`as?` 사용, 캐스팅에 실패하면 `nil`을반환하기 때문에 결과 타입은 옵셔널 타입
```swift
class Animal { }
class Cat: Animal { }

let animal: Animal = Cat()

let cat = animal as? Cat
```
`as?` 결과는 옵셔널 (Cat?)
## 3-2 강제 다운 캐스팅
`as!` 사용, 캐스팅에 실패하면 오류발생 / 성공하면 일반 타입 반환
```swift
let animal: Animal = Cat()

let cat = animal as! Cat
cat.meowong()
```
