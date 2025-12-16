# Server Component vs Client Component

- 10주 차 학습 : 2025.11.25 - 2025.11.28

## CSR의 한계

React는 기본적으로 CSR(Client Side Rendering) 방식이다.

### CSR 동작 과정

1. 브라우저가 서버에 페이지를 요청한다
2. 서버는 `<div id="root"></div>`만 있는 빈 HTML을 응답한다
3. 브라우저가 거대한 JS 번들 파일을 다운로드한다
4. JS 실행 후 React가 DOM을 생성하여 화면을 렌더링한다

### CSR의 문제점

- JS 다운로드가 완료되기 전까지 사용자는 흰 화면만 본다
- 인터넷 속도가 느린 환경에서는 사용자 경험이 매우 나쁘다
- 초기 로딩 시간이 길다

## Next.js의 해결책: Server Component

Next.js App Router는 Server Component를 기본값으로 사용한다.

### Server Component 동작 과정

1. 브라우저가 페이지를 요청한다
2. 서버에서 React 컴포넌트를 실행하여 HTML을 생성한다
3. 완성된 HTML을 브라우저에 응답한다
4. 사용자는 JS 로드 전에도 콘텐츠를 볼 수 있다
5. 필요한 최소한의 JS만 로드하여 Hydration 한다

## Server Component vs Client Component

### 비교표

| 특징          | Server Component     | Client Component                |
| ------------- | -------------------- | ------------------------------- |
| 선언 방식     | 기본값               | `'use client'` 명시             |
| 데이터 페칭   | `async/await` 사용   | `useEffect`, `React Query` 사용 |
| Hook 사용     | 불가능               | 가능                            |
| 이벤트 리스너 | 불가능               | 가능                            |
| 브라우저 API  | 접근 불가            | 접근 가능                       |
| JS 번들 크기  | 0 KB                 | 컴포넌트 크기만큼               |
| 주 사용처     | 데이터 조회, 정적 UI | 버튼, 폼, 상호작용              |

### Server Component를 사용하는 이유

**1) 번들 사이즈 최적화**

- 서버 컴포넌트 코드는 브라우저로 전송되지 않는다
- 무거운 라이브러리를 서버에서만 사용하면 사용자 다운로드 용량이 줄어든다

**2) 보안 강화**

- API Key, DB 비밀번호 같은 민감한 정보가 브라우저에 노출되지 않는다
- 서버 내부에서만 동작하기 때문에 안전하다

**3) 백엔드 직접 접근**

- DB나 파일시스템에 직접 접근할 수 있다
- 서버 내부에서 데이터를 조합하여 전달하므로 효율적이다

## 컴포넌트 선택 기준

### Server Component를 사용하는 경우

- 데이터베이스 직접 조회가 필요할 때
- API 호출이 필요할 때
- 정적인 UI를 렌더링할 때
- 민감한 정보를 다룰 때

### Client Component를 사용하는 경우

- `onClick`, `onChange` 같은 이벤트 리스너가 필요할 때
- `useState`, `useEffect` 같은 React Hook을 사용할 때
- `localStorage`, `window` 같은 브라우저 API를 사용할 때
- 사용자 상호작용이 필요한 UI일 때

## 주요 규칙

### Leaf 패턴 (나뭇잎 패턴)

클라이언트 컴포넌트는 컴포넌트 트리의 최하단에 위치시킨다.

```tsx
// 나쁜 예: 페이지 전체를 클라이언트 컴포넌트로 만듦
'use client';
export default function Page() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <Header />
      <Content />
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  );
}
```

```tsx
// 좋은 예: 상호작용이 필요한 부분만 클라이언트 컴포넌트로 분리
// page.tsx (Server Component)
export default function Page() {
  return (
    <div>
      <Header />
      <Content />
      <CounterButton />
    </div>
  );
}

// CounterButton.tsx (Client Component)
('use client');
export default function CounterButton() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

### Composition 패턴 (합성)

클라이언트 컴포넌트 안에 서버 컴포넌트를 넣을 때는 직접 import하지 않고 children으로 전달한다.

```tsx
// 나쁜 예: 클라이언트에서 서버 컴포넌트 직접 import
'use client';
import ServerComponent from './ServerComponent';

export default function ClientWrapper() {
  return <ServerComponent />;
}
```

```tsx
// 좋은 예: children으로 전달
// page.tsx (Server Component)
import ClientWrapper from './ClientWrapper';
import ServerComponent from './ServerComponent';

export default function Page() {
  return (
    <ClientWrapper>
      <ServerComponent />
    </ClientWrapper>
  );
}

// ClientWrapper.tsx (Client Component)
('use client');
export default function ClientWrapper({ children }) {
  const [isOpen, setIsOpen] = useState(false);
  return <div onClick={() => setIsOpen(!isOpen)}>{children}</div>;
}
```

### Props 직렬화

서버에서 클라이언트로 데이터를 전달할 때는 JSON으로 변환 가능한 데이터만 사용한다.

**가능한 타입**

- `string`
- `number`
- `boolean`
- `array`
- `object`

**불가능한 타입**

- `function`
- `Class` 인스턴스
- `Date` 객체

```tsx
// 가능한 예
<ClientComp
  name="John"
  age={25}
  isActive={true}
  tags={['react', 'next']}
  user={{ id: 1, name: 'John' }}
/>

// 불가능한 예
<ClientComp
  onClick={() => {}}
  date={new Date()}
  instance={new User()}
/>

// Date 객체는 문자열로 변환하여 전달
<ClientComp dateString={new Date().toISOString()} />
```

### 컴포넌트 구조

```
Server (page.tsx) - DB 조회
  ↓
Server (ProductList) - 리스트 렌더링
  ↓
Client (AddToCartButton) - 클릭 이벤트 처리
```

## 핵심 정리

- Next.js는 Server Component가 기본이다
- 상호작용이 필요한 부분만 `'use client'`를 사용한다
- 클라이언트 컴포넌트는 트리의 최하단에 위치시킨다
- 클라이언트 안에 서버를 넣을 때는 children 패턴을 사용한다
- Props로 전달할 수 있는 데이터 타입에 제한이 있다
- 서버가 할 수 있는 일은 서버가, 브라우저는 최소한만 담당한다
