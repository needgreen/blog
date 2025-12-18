# React 학습 정리

## 1. React 학습 전 필수 JavaScript 개념

### 변수 선언과 스코프

- **let, const, var**: `const`는 재할당 불가능한 상수, `let`은 블록 스코프 변수, `var`는 함수 스코프로 동작
- **클로저**: 함수가 선언될 때의 스코프를 기억하여 외부 함수의 변수에 접근할 수 있는 내부 함수

### 핵심 배열 메소드

React에서 리스트 렌더링 시 자주 사용하는 메소드들:

- `map()`: 각 요소를 변환해 새 배열 반환 (리스트 렌더링에 필수)
- `filter()`: 조건을 만족하는 요소만 추출
- `find()`: 조건을 만족하는 첫 번째 요소 반환
- `includes()`: 특정 값의 존재 여부 확인

### 구조 분해 할당과 스프레드 연산자

```jsx
// 배열 구조 분해
const [state, setState] = useState(0);

// 객체 구조 분해
const { name, age } = user;

// 스프레드(전개) 연산자
const newArray = [...oldArray, newItem];
const newObject = { ...oldObject, updated: true };
```

### 비동기 처리

- **Promise**: 비동기 작업의 성공/실패를 나타내는 객체
- **async/await**: Promise를 더 직관적으로 다루는 문법

---

## 2. React 기본 개념

### React와 ReactDOM

- **React**: UI를 구성하는 라이브러리
- **ReactDOM**: React Element를 실제 브라우저 DOM에 렌더링하는 라이브러리
- React 18부터 `ReactDOM.createRoot`를 통해 동시적 렌더링(concurrent rendering) 지원

### JSX (JavaScript XML)

HTML과 유사한 구조를 JavaScript 안에서 사용할 수 있는 문법:

```jsx
// 단일 루트 요소 필수
return (
  <div>
    <h1>Hello</h1>
    <p>{userName}</p>
  </div>
);

// Fragment 사용
return (
  <>
    <h1>Hello</h1>
    <p>{userName}</p>
  </>
);
```

**JSX 주요 특징**:

- `class` → `className`
- `onclick` → `onClick` (camelCase 방식)
- 중괄호 `{}`로 JavaScript 표현식 삽입
- 모든 태그는 닫는 태그 필수 (`<br />`, `<img />`)

### Component

컴포넌트는 독립적이고 재사용 가능한 UI 단위

```jsx
// 함수형 컴포넌트 (권장)
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

// 클래스형 컴포넌트 - React 16.8 이전에 주로 사용되던 컴포넌트 작성 방식
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## 3. Props와 State

### Props (Properties)

부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 읽기 전용 객체:

```jsx
// 부모 컴포넌트
<ChildComponent name="서정" age={25} />;

// 자식 컴포넌트
function ChildComponent({ name, age }) {
  return (
    <p>
      {name}님의 나이는 {age}세입니다.
    </p>
  );
}
```

**Props의 특징**:

- 읽기 전용 (불변성)
- 부모에서 자식으로 단방향 전달
- `defaultProps`로 기본값 설정 가능
- `props.children`으로 태그 사이 내용 접근State

컴포넌트 내부에서 관리되는 동적 데이터:

```jsx
// 함수형 컴포넌트
function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

## 4. React Hooks

### useState

함수형 컴포넌트에서 상태를 관리하는 기본 훅:

```jsx
const [state, setState] = useState(initialValue);

// 배열 반환: [현재 상태값, 상태 업데이트 함수]
```

### useEffect

사이드 이펙트(API 호출, 타이머 설정, DOM 조작 등)를 처리하는 훅:

```jsx
// 컴포넌트 마운트 시 1회 실행
useEffect(() => {
  console.log('컴포넌트가 처음 나타남');
}, []);

// 특정 값 변경 시 실행
useEffect(() => {
  console.log('count가 변경됨');
}, [count]);

// 정리(cleanup) 함수
useEffect(() => {
  const timer = setInterval(() => {}, 1000);

  return () => clearInterval(timer); // 컴포넌트 제거 시 실행
}, []);
```

### useReducer

복잡한 상태 관리를 위한 훅:

```jsx
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

const [state, dispatch] = useReducer(reducer, { count: 0 });

// 사용
<button onClick={() => dispatch({ type: 'increment' })}>+1</button>;
```

### useContext

전역 상태를 props drilling 없이 공유:

```jsx
const MyContext = React.createContext();

// Provider로 값 제공
<MyContext.Provider value={sharedData}>
  <ChildComponent />
</MyContext.Provider>;

// 자식 컴포넌트에서 사용
function ChildComponent() {
  const data = useContext(MyContext);
  return <div>{data}</div>;
}
```

### useMemo와 useRef

- **useMemo**: 연산 비용이 큰 계산 결과를 메모이제이션
- **useRef**: 렌더링과 무관하게 값을 유지하거나 DOM 요소에 직접 접근

```jsx
// useMemo
const expensiveValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);

// useRef
const inputRef = useRef(null);
const focusInput = () => inputRef.current.focus();

<input ref={inputRef} />;
```

## 5. React Router

### 기본 라우팅 구조

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

<BrowserRouter>
  <nav>
    <Link to="/">Home</Link>
    <Link to="/about">About</Link>
  </nav>

  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/about" element={<About />} />
  </Routes>
</BrowserRouter>;
```

### 동적 라우팅과 Hooks

```jsx
// 동적 라우트 정의
<Route path="/users/:id" element={<UserDetail />} />;

// useParams로 파라미터 가져오기
function UserDetail() {
  const { id } = useParams();
  return <div>User ID: {id}</div>;
}

// useNavigate로 프로그래밍 방식 이동
function LoginButton() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // 로그인 로직
    navigate('/dashboard');
  };
}
```

### 중첩 라우팅 (Nested Routing)

```jsx
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="about" element={<About />} />
  </Route>
</Routes>;

// Layout 컴포넌트
function Layout() {
  return (
    <div>
      <nav>...</nav>
      <Outlet /> {/* 자식 라우트가 렌더링될 위치 */}
    </div>
  );
}
```

## 6. 전역 상태 관리 실습

장바구니 기능을 구현하며 `useReducer`와 `useContext`를 활용한 전역 상태 관리를 학습했다.

### 상태 관리 구조

```
src/
├── context/
│   ├── cartContext.jsx    // Context 생성 및 Provider
│   └── cartReducer.js     // 상태 변경 로직
├── components/
│   ├── ProductList/       // 상품 목록
│   ├── ProductItem/       // 개별 상품
│   └── CartItem/          // 장바구니 아이템
└── pages/
    ├── Home/              // 홈 (상품 목록)
    └── Cart/              // 장바구니

```

### Reducer 기반 상태 관리의 장점

1. **로직 분리**: UI 컴포넌트와 상태 변경 로직이 분리됨
2. **재사용성**: 여러 컴포넌트에서 동일한 상태 변경 로직 사용
3. **유지보수**: 상태 변경 로직이 한 곳에 집중되어 관리 용이

## 7. 프로젝트 환경 설정

### Vite를 이용한 React 앱 생성

```bash
# 프로젝트 생성
npm create vite@latest my-app -- --template react

# 프로젝트 이동 및 실행
cd my-app
npm install
npm run dev

```

**Vite vs Create React App**:

- Vite가 개발 서버와 빌드 속도가 훨씬 빠름
- 신규 프로젝트는 Vite 사용 권장
- `.jsx` 확장자 필수

## 마치며

React는 컴포넌트 기반 아키텍처와 선언적 프로그래밍 방식으로 효율적인 UI 개발을 가능하게 한다. JavaScript 기본기를 탄탄히 다진 후 React의 핵심 개념들(Component, Props, State, Hooks, Router)을 순차적으로 학습하니 전체적인 구조가 명확하게 이해되었다.

특히 함수형 프로그래밍의 순수 함수와 불변성 개념이 React의 상태 관리와 긴밀하게 연결되어 있으며 `useReducer`와 `useContext`를 활용한 전역 상태 관리를 통해 복잡한 애플리케이션 구조도 체계적으로 관리할 수 있다.

다음 단계로는 실전 프로젝트를 통해 이러한 개념들을 더욱 깊이 있게 고민해보고 적용해보고자 한다.
