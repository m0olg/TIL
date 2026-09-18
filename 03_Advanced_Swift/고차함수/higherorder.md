# 고차함수
다른 함수를 전달인자로 받거나 함수실행의 결과를 함수로 반환하는 함수를 뜻함
## 종류
- `map` : 컨테이너 내부의 기존 데이터를 변형하여 새로운 컨테이너를 생성하는 것
- `filter` : 컨테이너 내부의 값을 걸러서 새로운 컨테이너로 추출
- `reduce` : 컨테이너 내부의 콘텐츠를 하나로 통합


### 예제
> 배열 준비
```swift
let capybaras = ["큰 카피바라", "작은 카피바라", "좀 더 큰 카피바라", "좀 더 작은 카피바라"]
```





- map
```swift
let cuteCapybaras = capybaras.map { "🐹 " + $0 }
print(cuteCapybaras)

== 결과

["🐹 큰 카피바라", "🐹 작은 카피바라", "🐹 좀 더 큰 카피바라", "🐹 좀 더 작은 카피바라"]
```
= 배열의 각 요소를 변형해서 새로운 배열 생성

- filter
```swift
let BBigOrBig = capybaras.filter {
    $0.contains("큰") || $0.contains("좀 더 큰")
}
print(happyOrBig)

== 결과

["큰 카피바라", "좀 더 큰 카피바라"]
```
= 조건에 맞는 요소만 걸러내서 새로운 컨테이너로 추출함

- reduce
```swift
let sentence = capybaras.reduce("카피바라들: ") {
    $0 + ", " + $1
}
print(sentence)

== 결과

"카피바라들: , 큰 카피바라, 작은 카피바라, 좀 더 큰 카피바라, 좀 더 작은 카피바라"
```
= 여러 값을 하나로 통합