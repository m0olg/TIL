# 01 print()
기본 출력!!, 단순 문자열을 출력함
```
// ex.

let name = "chicken feet"
let age = 100

print = (name, age)
```
보통 아래 설명할 dump 보단 **print를 주로 사용함**
# 02 dump()
**구조, 타입 확인** / 인스턴스의 자세한 설명까지 출력
(인스턴스 = 클래스나 구조체로부터 만들어진 객체의 값)

```
var name = "ddong"
dump(name)

// 출력 
- some: "ddong"
/* some은 옵셔널 안에 들어있는 값의 이름 */
```

---

### ++ debugprint()
→ 디버깅용 print보다 자세히 출력 가능 
# 03 print와 dump의 차이 (예시)

코드 ⬇️


![alt text](image-2.png) 

출력값 ⬇️⬇️⬇️

![alt text](image-3.png)

→ 보이는 거와 같이 *print는 사람이 읽기 좋게 간단히 출력* 하고, *dump는 구조와 타입까지 포함해 디버깅용으로 자세히 출력* 함!