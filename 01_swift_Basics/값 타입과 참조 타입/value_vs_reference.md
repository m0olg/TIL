## < 값 타입과 참초 타입>

### 각각의 특징
<details>
<summary>class</summary>

- 단일 상속
- 참조 타입
- 큰 뼈대는 모두 class로 구성
- 참조 복사

</details>

<details>
<summary>Struct (구조체)</summary>

- 상속 불가
- 값 타입
- 큰 뼈대는 모두 구조체로 구성
- 값 복사

</details>

<details>
<summary>Enum (열거형)</summary>

- 상속 불가
- 값 타입
- 열거형 자체가 하나의 데이터 타입으로 열거형의 case 하나하나 전부 하나의 유의미한 값으로 취급 ‼️
- 값 복사
</details>


### 표로 한 눈에 정리

|    | class | Struct | Enum |
|:---|:-----:|:-------:|:------:|
|Type | Reference (참조) | Value (값) | Value (값) |
Subclassing | ⭕ |❌ |❌ |
Extension | ⭕ | ⭕|⭕|
capy | 침조 복사 | 깂 복사 | 값 복사







### ++
#### <언제 뭐를 쓸까>
- struct : 데이터 묶음 (기본 선택)
- class : 공유/참초 필요할 때
- enum : 상태/경우의 수 표현

#### <메모리 관점>
- class → heap 저장 + 참조 전달
- struct/enum → stack 저장 (빠름, 안전)