### List
# 01 List란?
→ **여러개의 행을 표현하기 위한 뷰이며 단일 열에 정렬 된 데이터 행을 표시하는 컨테이너**

List로 정적 컨텐츠 표현 \
(정적 컨텐츠란? \
코드에 '이미 고정되어 있는 UI' = 실행 중에 바뀌지 않고 개수가 미리 정해진 뷰)

## 1-1 List로 정적 콘텐츠 표현하기
```swift
List {
    Text("hello")
    Text("world")
}
```
뷰 빌더의 제약 때문에 정적 컨텐츠 표현시 중괄호 안에 10개 넘는 뷰를 집어넣으면 에러 뜸

원하는 뷰를 전달하면 하나씩 각 `row`에 담아 표현 \
여기서 뷰 하나는 `row`하나에 표현

이미지를 넣어도 작동 함

## 1-2 List로 동적 콘텐츠 표현하기

### ① `Range<Int>`
- 동적 컨텐츠를 표현하는 첫번째 방법은 `Range<Int>` 타입의 값을 넘겨주는 것
```swift
List(o..<100) {
    Text("\($0)")
}
```

### ② `RandomAccessCollection`
- 두 번째는 `RandomAccessCollecion` 프로토콜을 준수하는 데이터를 제공하는 것

    1. `id` 식별자 지정 \
    사용할 값을 인수로 직접 제공, 스유에서는 이것을 간단히 `self`라고 입력
    ```swift
    .List(["A", "B", "C", "D"], id: \.self) { ... }
    ```
    2. `identifiable` 프로토콜 채택 \
    전달하는 대신 데이터 타입 자체에 `identifiable` 프로토콜을 채택하는 것, 타입 자체에 `id`프로퍼티를 만들고 이것을 식별자로 삼음

    `Ghibli` 라는 `struct`을 만듦 → `Identifiable`을 매개변수 타입으로 넣어주기 →
    상수와 식별가능한 코드 `id = UUID()`를 작성
    ```swift
    struct Ghibli: Identifiable {
    let name: String
    let id = UUID()
    }
    private var ghibli = [
      Ghibli(name: "닭"),
      Ghibli(name: "발"),
      Ghibli(name: "먹"),
       Ghibli(name: "고"),
       Ghibli(name: "싶"),
       Ghibli(name: "다")
    ]
    ```

- `identifiale` 프로토콜을 준수한다면 이미 식별자가 있으므로 리스트에 제공하지 않아도 무방함


# 02 정적 콘텐츠와 동적 콘텐츠의 조합
`ForEach`사용하면 조합 가능

```swift
struct ContentView: View {
    
    let dakbalMenu = ["국물 닭발", "직화 닭발", "치즈 닭발", "무뼈 닭발"]
    let sideMenu = ["주먹밥", "계란찜", "쿨피스"]
    
    var body: some View {
        List {
            Text("닭발 메뉴")
            
            ForEach(dakbalMenu, id: \.self) {
                Text("\($0)")
            }
            
            Label("사이드 메뉴", systemImage: "flame.fill")
                .font(.largeTitle)
            
            ForEach(sideMenu, id: \.self) {
                Text("\($0)")
            }
        }
    }
}
```

# 03 `Section`
→ 섹션에는 `header`와 `footer`를 생략하거나 추가할 수 있고 둘 중에 하나만 사용 ⭕️ \
```swift
struct ContentView: View {
    
    let drink = ["카피한카피바라", "표절한카피바라", "날좀봐라카피바라", "도용한카피바라"] // 목록 배열
    
    let snack = ["직화닭발", "무뼈닭발", "치즈닭발"] // 목록 배열
    
    var body: some View {
        
        // 섹션 제목 배열 (데이터의 카테고리)
        let titles = ["음료수", "과자"]
        
        // 실제 표시할 데이터
        let data = [drink, snack]
        
        return List {
            
            // data 배열의 인덱스 개수만큼 반복 → 섹션 생성
            ForEach(data.indices) { index in
                
                Section(
                    // 섹션 헤더: 카테고리 이름 표시
                    header: Text(titles[index]),
                    
                    // 섹션 푸터: 해당 항목 개수 표시
                    footer: HStack {
                        Spacer() // 오른쪽 정렬을 위한 여백
                        Text("\(data[index].count)건") // 항목 수 표시
                    }
                ) {
                    
                    // 각 섹션 내부의 아이템 반복
                    ForEach(data[index], id: \.self) {
                        
                        // 각 항목을 아이콘, 텍스트 형태로 표시
                        Label($0, systemImage: "leaf.fill")
                        
                        // 기본 텍스트로 표시하고 싶으면 아래 코드 사용
                        // Text($0)
                    }
                }
            }
        }
        
        // 리스트 스타일 지정
        // .listStyle(GroupedListStyle())
    }
}
```

# 04 `ListStyle`
→ `ListStyle`을 사용하기 위해선 `List { }` / `.listStyle()` 처럼 리스트 밖에 스타일을 추가해줘야함
```swift
List {  ... }
  .listStyle(DefaultListStyle())
```

<details>
<summary>ListStyle의 종류</summary>


- `DefaultListStyle()` ➜ 기본 스타일 리스트
- `GroupedListStyle()` ➜ 각 섹션을 분리된 그룹으로 묶어 표현
- `InsetGroupedListStyle()`
- `PlainListStyle()` ➜ 데이터 목록을 각 행마다 하나씩 나열하는 기본 스타일
- `InsetListStyle()`
- `SidebarListStyle()`

</details>

```swift
// 하나의 행(Row)을 나타내는 뷰
struct TaskRow: View {
    var body: some View {
        // 간단한 텍스트 표시
        Text("Hello, World!")
    }
}

// 메인 콘텐츠 뷰
struct ContentView: View {
    
    var body: some View {
        // 리스트 생성 (스크롤 가능한 테이블 형태)
        List {
            
            // 첫 번째 섹션
            Section(
                header: Text("Header"),   // 섹션 상단 제목
                footer: Text("footer"),   // 섹션 하단 설명
                content:  {
                    TaskRow()
                    TaskRow()
                    TaskRow()
                }
            )
            
            // 두 번째 섹션
            Section {
                TaskRow()
                TaskRow()
                TaskRow()
            }
        }
        // 리스트 스타일 지정
        .listStyle(DefaultListStyle())
    }
}
```

# 05 `onDelete`, `onMove`
`List` 내부에있는 텍스트를 옮기거나 삭제 ⭕️ \
이걸 사용하려면 `.onDelete(perform: )`, `.onMove(perform: )`를 사용하고 함수를 정의 해줘야함

### ⬇️⬇️ `onDelete`
```swift
 func removeList(at offsets: IndexSet) {
        users.remove(atOffsets: offsets)
    }
```
### ⬇️⬇️ `onMove`
```swift
    func moveList(from source: IndexSet, to destination: Int) {
        users.move(fromOffsets: source, toOffset: destination)
    }
```
- 이 코드를 사용하기 위해서는 `List`를 `NavigationView` 로 감싸고, 그 옆에 `EditButton`을 추가해줘야 함 \
이 버튼이 없으면 `Text`를 슬라이스 해서 삭제는 할 수 있지만 `Move`는 할 수 ❌

# 06 Selection in Lists
- 목록 데이터의 `Identifiable` 단일 인스턴스에 바인딩
- `id`유형은 단일 선택 목록 생성
- `set`에 바인딩하면 여러 선택 항목을 지원하는 목록이 생성

1. `@state`변수 추가
```swift
@State private var multiSelection = Set<UUID>()
```

2. NavigationView를 생성 후 `List`에 `multiSelection`을 추가

3. NavigationBarTitle을 이용하여 이름을 설정해주고, 그 옆에 `.toolbar { EditButton() } `을 추가해 선택 가능한 버튼 만들기

# 07 리스트 여백 설정
- `List`에는 기본적으로 `padding`이 들어가 있음 \
여백을 설정하고 싶으면 `listRowInsets` 를 사용하면 됨

```swift
.listRowInsets(EdgeInsets.init())

.listRowInsets(EdgeInsets.init(top: CGFloat, leading: CGFloat, bottom: CGFloat, trailing: CGFloat))
```