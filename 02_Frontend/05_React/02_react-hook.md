# React Hook

### React.useState()

- 함수형 컴포넌트에서 **상태(state)를 선언하고 관리**하기 위한 기본적인 훅
- 컴포넌트에서 변경 가능한 데이터를 저장하고 관리할 때 사용
- 기본구조

  ```jsx
  const [state, setState] = useState(initialValue);

  - state - 상태 변수의 이름
  - setState - 상태 변수 값을 변경하는 상태 업데이트 함수
  - initialState - 상태 변수의 초기값
  ```

- useState()는 배열을 반환
  - 첫 번째 요소는 현재 상태 값 `state`
  - 두 번째 요소는 상태 값을 변경하는 함수 `setState`
- **컴포넌트 최상위에서만 호출** : 조건문, 반복문, 중첩 함수 안헤서 호출하면 안됨.
- **불변성 유지** : 배열이나 객체를 직접 수정하지 않고 새로운 값을 생성해야 함.

### React.useEffect()

- 함수형 컴포넌트에서 사이드 이펙트(side effect)를 처리하기 위한 훅
  - 사이드 이펙트 : 컴포넌트가 화면에 보이는 것 외에 해야 하는 작업들
    - 예) 데이터 가져오기(API 호출), 타이머 설정, 직접 DOM 조작, 구독 설정
- 기본 구조

  ```jsx
  useEffect(() => {
    // 실행할 코드

    return () => {
      // 정리(cleanup) 코드 (선택사항)
    };
  }, [의존성 배열]);
  ```

- `React.useEffect(setup[, dependencies]);`
  - setup(필수) : 사이드 이펙트가 동작할 코드가 작성된 함수
    - setup 함수는 `cleanup` 함수를 반환할 수 있음
    - **정리(cleanup) 함수**
      - 컴포넌트가 사라지거나 effect가 다시 실행되기 전 정리 작업이 필요할 때 사용
      - cleanup 함수가 실행되는 시점
        - 컴포넌트가 화면에서 제거될 때(Unmount)
        - 의존성 배열의 값이 변경되어 effect가 다시 실행되기 직전
  - dependencies (선택)
    - 의존성 배열(`[]`) ⇒ **언제 effect를 다시 실행할 지 결정**
- 의존성 배열에 따른 사이드 이펙트 실행 시점
  - 의존성 배열이 없는 경우 ⇒ 컴포넌트가 **렌더링 될 때마다 실행** (매번 실행)
    ```jsx
    useEffect(() => {
      console.log('컴포넌트가 렌더링될 때마다 실행');
    });
    ```
  - 의존성 배열이 빈 배열`[]` 인 경우 ⇒ 컴포넌트가 처음 마운트 될 때 **1회만 실행**
    ```jsx
    useEffect(() => {
      console.log('컴포넌트가 처음 나타날 때만 실행');
    }, []);
    ```
  - 의존성 배열에 특정 값이 있는 경우 ⇒ 해당 값이 **변경될 때마다 실행**
    ```jsx
    useEffect(() => {
      console.log('count가 변경될 때만 실행');
    }, [count]);
    ```
- 무한 루프 주의 : 의존성 배열에 잘못된 값을 넣으면 무한 루프 발생
- async/await 직접 사용 금지 : useEffect의 콜백 함수는 async로 생성 불가

### React.useReducer()

- 함수형 컴포넌트에서 복잡한 상태 관리를 할 때 사용하는 훅
  - 여러개의 하위 값을 포함할 때
  - 다음 상태가 이전 상태에 의존할 때
  - 상태 업데이트 로직이 복잡할 때
  - **여러 곳에 재사용해야 할 때**
- 기본 구조

  ```jsx
  const [state, dispatch] = useReducer(reducer, initialState);

  state: 현재 상태값
  dispatch: 상태를 변경하기 위해 호출하는 함수
  reducer: 상태를 어떻게 변경할지 정의한 함수
  initialState: 초기 상태값

  dispatch: action(행동 명령)을 reducer에 전달하는 함수
  ```

- 예시

  ```jsx
  import { useReducer } from 'react';

  // reducer 함수: 어떤 동작(action)을 할지 정의
  function reducer(state, action) {
    switch (action.type) {
      case 'increment':
        return { count: state.count + 1 };
      case 'decrement':
        return { count: state.count - 1 };
      default:
        return state;
    }
  }

  function Counter() {
    // 초기값은 { count: 0 }
    const [state, dispatch] = useReducer(reducer, { count: 0 });

    return (
      <div>
        <p>카운트: {state.count}</p>
        <button onClick={() => dispatch({ type: 'increment' })}>+1</button>
        <button onClick={() => dispatch({ type: 'decrement' })}>-1</button>
      </div>
    );
  }
  ```

- **`type`** → “무슨 행동을 할 건지” (명령 이름)
- **`payload`** → “그 행동을 위해 필요한 데이터” (명령의 인자)
- **`dispatch()`** → 이 두 정보를 묶어서 `reducer`에게 전달하는 실행 트리거
- `swith(action.type)` 의 `case` 와 `dispatch` 의 `type` 값은 반드시 동일하게 사용.

### React.useContext()

- 컴포넌트 트리 전체에서 데이터를 공유하기 위한 Context 객체를 만드는 함수
  - props를 일일이 전달하지 않고도 하위 컴포넌트들이 데이터를 사용할 수 있음.
- `React.createContext()` 기반으로 작동하며 세트로 사용
  - 컴포넌트 분기 시 자식간 데이터 전달은 불가 / 부모 컴포넌트(공통)에서 자식 컴포넌트(상태 변수)로 전달
- React.createContext() 구문

  ```jsx
  const context = React.createContext(defaultValue);
  ```

  - 컨텍스트 객체에는 Provider와 Consumer 컴포넌트가 포함

    ```jsx
    const MyContext = React.createContext();

    // MyContext 안에는 두 가지 중요한 컴포넌트 생성
    // → MyContext.Provider
    // → MyContext.Consumer
    ```

    - `Provider`는 **데이터를 공급(Provide)** 하는 역할, Context의 값을 설정해서 하위 컴포넌트에 전달
    - `Consumer`는 데이터를 소비(Consume) 하는 컴포넌트로 `Provider`가 전달한 값을 읽을 때 사용

- React.useContext()
  - Context 데이터를 쉽게 꺼내 쓰기 위한 React Hook
  - `Consumer` 컴포넌트를 대신해서 **Context의 현재 값을 직접 가져올 수 있게 해주는 함수**
  - 가장 가까운 `Provider`의 `value` 값을 반환
  - 함수형 컴포넌트 안에서만 사용 가능
  - Context API를 통해 전역 데이터(예: 테마, 사용자 정보 등)를 props 없이 여러 컴포넌트에서 공유할 때 사용
    | 방식 | 설명 | 코드 예 |
    | -------------- | ----------------------------------- | ----------------------------------------------------------- |
    | **Consumer** | JSX에서 render props 형태로 값 접근 | `<MyContext.Consumer>{(value) => ...}</MyContext.Consumer>` |
    | **useContext** | Hook으로 직접 값 접근 (간결) | `const value = useContext(MyContext)` |

### React.useMemo()

- 연산 비용이 큰 계산 결과를 메모이제이션(Memoization) 해서 불필요한 재계산을 방지하는 훅
  - 의존성 배열에 명시된 값이 변경되지 않으면 이전에 계산된 값을 재사용
  - 렌더링 최적화 ⇒ 불필요한 연산을 줄이고 렌더링 성능 향상
  - 객체나 배열과 같은 참조형 데이터를 useMemo로 감싸면 리렌더링 시에도 동일한 참조를 유지
- React.useMemo() 구문
  ```jsx
  const memoizedValue = React.useMemo(calculateValue, [dependencies]);
  ```
  - `calculateValue` **: 계산을 수행하는 함수** (보통 `() => {...}` 형태로 작성됨)
  - `[dependencies]` : 의존성 배열 — 이 안의 값이 바뀔 때만 calculateValue가 다시 실행됨
  - `memoizedValue` : 메모된 결과(즉, `calculateValue`의 반환값)
- **컴포넌트가 렌더링될 때** React는 `useMemo()`를 확인하고
  → “이전에 저장된 값이 있고, dependencies가 안 바뀌었네?”
  → 그럼 이전 계산 결과를 재사용!
  - **의존성이 바뀌면**
    - `calculateValue` 함수가 다시 실행되고
    - 그 **새 결과를 다시 저장(memo)** 함
- `() => calculateValue()`처럼 **함수 형태로 전달**해야 함
- useMemo()는 값을 메모이제이션해서 의존성이 바뀔 때만 다시 계산하고 그 결과를 캐싱해두는 훅

### React.useRef()

- 컴포넌트가 다시 렌더링되어도 값이 유지되는 ‘상자(container)’를 만드는 훅
- 리렌더링을 발생시키지 않고 **값을 기억하거나 DOM 요소를 직접 제어할 때** 사용
- 기본 문법
  ```jsx
  const ref = React.useRef(initialValue);
  ```
  - `initialValue` : 초기값 (보통 null 또는 원하는 기본값)
  - `ref` : 반환 객체 ref는 `{ current: initialValue }` 형태
  - `ref.current` : 실제로 저장되는 값 (자유롭게 변경 가능)

# ✅ 라우팅(Routing)이란?

사용자가 요청한 URL에 따라 보여줄 화면(페이지)을 바꿔주는 것

- 어떤 경로(path, 예: `/home`, `/about`)로 들어왔는지에 따라 어떤 화면을 보여줄지 결정하는 과정
- react-router-dom 라이브러리
- 설치 : `npm install react-router-dom`
- 주요 컴포넌트
  | 컴포넌트 | 역할 | 주요 기능 |
  | -------------------------- | ----------------------- | ---------------------------- |
  | **BrowserRouter** | 전체 라우팅 감싸는 루트 | SPA 구조 유지 |
  | \* Single Page Application |
  | **Routes** | Route들의 묶음 | 한 번에 하나의 라우트만 표시 |
  | **Route** | 경로와 컴포넌트 연결 | path와 element 지정 |
  | **Link** | 새로고침 없는 이동 | 내부 네비게이션 |
  | **useNavigate()** | 코드로 페이지 이동 | 버튼 클릭 등 이벤트 이동 |
  | **useParams()** | URL 파라미터 가져오기 | 동적 라우팅 지원 |
  | **Outlet** | 중첩 라우트 위치 지정 | 부모-자식 관계 라우팅 구현 |

### Routes

`Route`들을 감싸서 한 번에 관리

```jsx
import { Routes, Route } from 'react-router-dom';

<Routes>
  <Route path="/about" element={<About />} />
  <Route path="/contact" element={<Contact />} />
</Routes>;
```

### Route

경로(URL)와 컴포넌트를 연결하는 컴포넌트

```jsx
<Route path="/about" element={<About />} />
```

| 속성       | 설명                               |
| ---------- | ---------------------------------- |
| `path`     | URL 경로 지정                      |
| `element`  | 해당 경로일 때 렌더링할 컴포넌트   |
| `index`    | 기본 경로 지정 시 사용 (`/` 대신)  |
| `children` | 중첩 라우팅(Nested Routing)에 사용 |

### Link

새로고침 없이 페이지를 이동할 수 있게 해주는 링크

- 페이지 새로고침(깜빡임) 없이 리액트 라우터가 내부적으로 컴포넌트만 교체

```jsx
import { Link } from 'react-router-dom';

<Link to="/about">About 페이지로 이동</Link>;
```

### Outlet

중첩 라우팅(Nested Routing)을 할 때 부모 컴포넌트 안에서 자식 라우트를 렌더링할 위치를 지정하는 컴포넌트

- 라우트 내 라우트를 구성하는 것 = 중첩 라우트

```jsx
import { Outlet, Link } from 'react-router-dom';

function Layout() {
  return (
    <div>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>
      <hr />
      <Outlet /> {/* 여기 자식 라우트들이 렌더링됨 */}
    </div>
  );
}
```

```jsx
<Routes>
  {/* 자식 라우트가 있는 경우 <Route></Route>로 사용 */}
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="about" element={<About />} />
    {/* 자식 라우트의 path에는 /를 붙이지 않음 */}
  </Route>
</Routes>
```

- **`Outlet`에 Props 전달하기 (Context 형태)**

| 구분        | 설명                                                          |
| ----------- | ------------------------------------------------------------- |
| 전달 방식   | 부모의 `<Outlet context={{...}} />`                           |
| 받는 방식   | 자식에서 `useOutletContext()` 사용                            |
| 데이터 형태 | 객체, 배열, 함수 등 어떤 형태도 가능                          |
| 사용 목적   | 부모 컴포넌트의 공통 데이터 공유 (예: 사용자 정보, 설정값 등) |
| 스코프      | 해당 `<Outlet />` 아래의 자식 라우트에만 전달됨               |

## 동적 라우팅

동적 세그먼트( 동적 파라미터, 경로 변수)를 통해 URL에 따라 컴포넌트가 동적으로 렌더링

- 문법 : `/경로/:변수명`

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/users/:id" element={<UserDetail />} /> {/* 동적 라우트 */}
</Routes>
```

- **useParams()** 훅

  - `Route` 경로에서 `:id`처럼 정의된 **파라미터값을 추출**
  - `useParams()`로 값 가져오기
  - 주로 상세 페이지나 특정 리소스 접근 시 사용

- `location.href`는 자바스크립트의 브라우저 내장 객체 `window.location`의 속성을 이용해 현재 페이지의 URL을 변경하는 방법 ⇒ 깜빡거림 발생
  ```jsx
  <button onClick={() => (location.href = '/about')}>소개 페이지</button>
  ```
- **useNavigate()** 훅

  - 페이지를 새로고침 없이 이동시킬 수 있게 해주는 함수
  - 페이지 전체를 새로 로드하지 않고 컴포넌트만 교체

    ```jsx
    // 기본 이동
    navigate('/about');

    navigate(-1); // 이전 페이지로 이동
    navigate(1); // 다음 페이지로 이동

    // window.history.back() / window.history.forward() 와 유사하지만
    // 리액트 라우터가 관리하는 SPA 히스토리 안에서 동작
    ```

## 쿼리 스트링(Query String)

URL 주소 끝에 붙는 추가적인 데이터 전달 - 주로 `?` 기호 뒤에 `key=value` 형태

```jsx
https://example.com/search?keyword=apple&page=2

// ? -> 쿼리 스트링 시작
// keyword=apple -> key: keyword, value: apple
// page=2 -> key: page, value: 2
// & -> 여러 값을 구분 할 때 사용
```

- **useSearchParams()**
  - URL의 쿼리 스트링(예: ?page=2&keyword=apple)을
    읽고(`get`) 수정(`set`)할 수 있게 해주는 기능
- **searchParams**
  - `useSearchParams()`로부터 반환되는 첫 번째 값
  - 현재 URL에 들어있는 쿼리 스트링 값들을 읽을 수 있는 객체
  - 자주 쓰는 메소드
    - `.get(key)` : searchParams.get("nnn") ⇒ “n”
    - `.getAll(key)` : searchParams.getAll("nnn") ⇒ [ ”nn”, “nnn” .. ]
    - `.has(key)` : searchParams.has("nn") ⇒ true/false
    - `.toString()` : 전체 쿼리를 문자열로 변환
    - `.entries()` : for…of 로 순회

## 전역 상태 관리 실습

1. 컴포넌트 구조 생각

   ```jsx
   src/
   |---data/
   |  |--products.json
   |
   |---components/
   |  |--ProductList
   |  |--ProductItem
   |  |--CartItem
   |
   |---pages/
   |  |--Cart
   |  |--Home
   |
   |---context/
   |  |--cartContext
   |  |--cartReducer
   |
   |---App
   |---main
   |
   ```

2. 상태관리 해야 되는게 무엇일지 생각
   - 장바구니
     - 장바구니 추가
     - 수량 버튼
     - 금액 합산
     - 개별 삭제
     - 전체 삭제
     - 장바구니 비어있을 때 안내메세지
     - 구매하기 버튼
   - 상품
     - 아이템 목록
     - 장바구니 담기
3. 상태관리 해야 되는 데이터의 구조 생각
   - 상품 데이터
   - 장바구니 상태
   -
4. 상태 변경이 발생되는 경우들 생각
   - 상품 추가 → 장바구니 추가 클릭
   - 수량 변경 → 장바구니에서 +/- 버튼 클릭
   - 상품 삭제 → 장바구니에서 삭제 버튼 클릭
   - 전체 삭제 → 장바구니 비우기
   - 주문하기 → 주문하기 버튼 클릭 → 장바구니 초기화+안내메세지
   - 총 금액 → 상품 추가, 수량 변경, 삭제에 따라 합산 금액 표시
5. 각 경우별 상태 변경 로직 흐름 생각
   1. product item 컴포넌트
   2. 장바구니 추가 버튼
   3. cartReducer 실행
   4. 추가된 상품인지 확인
   5. 수량 + 1
   6. 총 금액 계산
   7. 상태 업데이트
   8. cart 컴포넌트 리렌더링

**상태 변경 로직 흐름**

app ⇒ Context.Provider value=상태, 상태업데이트 함수 정의

cart ⇒ useContext 로 꺼내서 사용 가능

- CartItem ⇒ 장바구니 전체 삭제, 아이템 수량 증가/감소, 아이템 삭제

productlist ⇒ useContext 로 꺼내서 사용 가능

- ProductItem

**상태 업데이트 함수**

1. 기존 아이템인지 확인

2. 신규일 경우 신규 아이템으로 추가

3. 기존 아이템인 경우 수량만 증가

**상태 관리 방법**

1. useState + useContext ⇒ 각 컴포넌트마다 상태 변경 로직(복잡) 구문을 작성
2. useReducer + useContext

- 각 컴포넌트마다 상태변경 로직 작성 안해도 됨.
- 컴포넌트 ui/상태 변경 로직 분리
