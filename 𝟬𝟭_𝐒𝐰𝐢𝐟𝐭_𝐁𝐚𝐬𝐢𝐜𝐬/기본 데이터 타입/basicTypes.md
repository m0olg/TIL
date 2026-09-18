# 01 숫자 타입
## 1-1 정수형
→ 정수형은 **소수점이 없는 숫자를 표현하는 데이터 타입**

### 종류
- ```Int```:  기본 정수형 타입
- ```Int8, Int16, ⋯ Int64 ```: 8, 16, ⋯ 64비트 정수
- ```Ulnt```: 양의 정수 타입, 현재는 기본적으로 64비트 양의 정수형
```swift
let age: Int = 25
let maxUInt8: UInt8 = 255 // 부호없는 8비트: 0~255
let minInt8: Int8 = -128 // 부호있는 8비트: -128~127
```

## 1-2 실수형
→ 실수형은 소수점이 있는 숫자를 표현하는 데이터 타입

### 종류
- ```Float```: 32비트 부동소수점 (소수점 아래 약 6자리까지 정확도 보장)
- ```Double```: 64비트 부동소수점 (소수점 아래 약 15자리까지 정확도 보장)
```swift
let height: Float = 144.4 // 32비트: 6자리 정확도
let pi: Double = 3.14159265359 // 64비트: 15자리 정확도
```

# 02 문자와 문자열
## 2-1 Character (문자)
→ Character는 **단일 문자를 표현**하는 타입으로, 글자, 숫자, 특수문자, 이모지 등 하나의 문자를 저장 ⭕️
```swift
let grage : Character = "A"
let heart: Character = "♥️" // 이모지도 하나의 문자로 취급
let 한글: Character = "가" // 한글도 가능!!
```
## 2-2 String (문자열)
### 특징
- 유니코드 문자열을 완벽하게 지원
- 문자열 보간법 지원
- 다중 행 문자열 지원
```swift
// 기본 문자열
let name: String = "Swift"

// 여러 줄 문자열
let multiLine = """
여러 줄의
문자열을
작성할 수 있다
"""

// 문자열 보간법
let language = "Swift"
let version = 5
let info = "\\(language) \\(version).0" // "Swift 5.0"
```
# 03 Bool
→ Bool은 **참(True) 또는 거짓(false) 두 가지 값만 가질 수 있는** 논리 데이터 타입
```swift
let kku: Bool = true
let kuu: Bool = false

// 조건문에서 주로 사용
if kku {
    print("사용 가능합니다")
}
