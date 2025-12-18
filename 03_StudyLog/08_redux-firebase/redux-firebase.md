# 리덕스 툴킷, 파이어 베이스

## 1. Redux Toolkit이란?

### Redux의 문제점과 Redux Toolkit의 등장

기존 Redux는 강력하지만 보일러플레이트 코드가 많고 설정이 복잡했다. Redux Toolkit(RTK)은 Redux 공식 팀이 만든 **Redux를 더 쉽고 효율적으로 사용하기 위한 도구 모음**이다.

**Redux Toolkit의 장점**:

- 보일러플레이트 코드 대폭 감소
- 불변성 관리 자동화 (Immer 내장)
- 간편한 비동기 처리 (createAsyncThunk)
- Redux DevTools 자동 설정
- 모범 사례가 기본값으로 설정됨

### 설치

```bash
npm install @reduxjs/toolkit react-redux
```

## 2. Redux Toolkit 핵심 개념

### Store 생성

```jsx
*// src/store/store.js*
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';
import userReducer from './userSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
});
```

### Slice 생성

Slice는 **특정 기능에 대한 리듀서와 액션을 한 곳에 정의**하는 Redux Toolkit의 핵심 개념이다.

```jsx
*// src/store/counterSlice.js*
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
  },
  reducers: {
    increment: (state) => {
      *// Immer 덕분에 직접 수정하는 것처럼 작성 가능*
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

*// 액션 크리에이터 자동 생성*
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

*// 리듀서 내보내기*
export default counterSlice.reducer;
```

### Provider로 앱 감싸기

```jsx
*// src/main.jsx*
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './store/store';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

### 컴포넌트에서 사용하기

```jsx
*// src/components/Counter.jsx*
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, incrementByAmount } from '../store/counterSlice';

function Counter() {
  *// 상태 읽기*
  const count = useSelector((state) => state.counter.value);

  *// 액션 디스패치*
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => dispatch(increment())}>+1</button>
      <button onClick={() => dispatch(decrement())}>-1</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>+5</button>
    </div>
  );
}
```

## 3. 비동기 처리 - createAsyncThunk

API 호출 같은 비동기 작업을 처리하는 Redux Toolkit의 강력한 기능

```jsx
*// src/store/userSlice.js*
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

*// 비동기 액션 생성*
export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId) => {
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
  }
);

const userSlice = createSlice({
  name: 'user',
  initialState: {
    data: null,
    loading: false,
    error: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  },
});

export default userSlice.reducer;
```

**사용 예시**:

```jsx
function UserProfile({ userId }) {
  const dispatch = useDispatch();
  const { data, loading, error } = useSelector((state) => state.user);

  useEffect(() => {
    dispatch(fetchUser(userId));
  }, [userId, dispatch]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!data) return null;

  return <div>Hello, {data.name}</div>;
}
```

## 4. Firebase란?

Firebase는 Google이 제공하는 **모바일 및 웹 애플리케이션 개발 플랫폼**으로, 백엔드 인프라를 직접 구축하지 않고도 다양한 기능을 빠르게 구현할 수 있다.

### Firebase 주요 서비스

| 서비스             | 분류               | 주요 용도                     | 설명                                                                                                                           |
| ------------------ | ------------------ | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Authentication** | 사용자 인증        | 로그인, 회원가입, 소셜 로그인 | 이메일·비밀번호, 전화번호, Google·Apple·Facebook 등 소셜 계정을 이용한 간편 인증을 제공하며, 안전한 인증 흐름을 구축할 수 있음 |
| **Firestore**      | NoSQL 데이터베이스 | 실시간 데이터 저장 및 동기화  | 문서·컬렉션 기반 구조를 사용하며 실시간 업데이트가 가능해 채팅, 알림, 협업 앱 등에 적합                                        |
| **Storage**        | 파일 저장소        | 이미지, 동영상 등 파일 업로드 | 대용량 파일 저장을 지원하며, 보안 규칙(Security Rules) 기반 접근 제어 가능                                                     |
| **Hosting**        | 웹 호스팅          | 정적 사이트 배포              | React, Vue, Next.js 등 프론트엔드 앱을 쉽고 빠르게 배포할 수 있으며 CDN 기반으로 빠른 응답 속도 제공                           |
| **Functions**      | 서버리스 함수      | 백엔드 로직 실행              | 서버 관리 없이 백엔드 코드를 실행할 수 있으며, 인증 이벤트·DB 업데이트·HTTP 요청 등에 반응하여 자동 처리 가능                  |

## 5. Firebase 시작하기

### Firebase 프로젝트 설정

1. [Firebase Console](https://console.firebase.google.com/) 접속
2. 새 프로젝트 생성
3. 웹 앱 추가 (`</>` 아이콘 클릭)
4. Firebase 설정 정보 복사

### Firebase SDK 설치

```bash
npm install firebase
```

### Firebase 초기화

```jsx
*// src/firebase/config.js*
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';
import { getStorage } from 'firebase/storage';

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

*// Firebase 초기화*
const app = initializeApp(firebaseConfig);

*// 서비스 인스턴스 생성*
export const auth = getAuth(app);
export const db = getFirestore(app);
export const storage = getStorage(app);
```

## 마치며

Redux Toolkit과 Firebase를 함께 사용하면 **상태 관리**와 **백엔드 인프라**를 빠르게 구축할 수 있다.

**주요 학습 포인트**:

- Redux Toolkit의 `createSlice`와 `createAsyncThunk`로 간결한 상태 관리
- Firebase Authentication으로 손쉬운 사용자 인증
- Firestore로 실시간 NoSQL 데이터베이스 구현
- 두 기술의 활용으로 효율적인 전역 상태 관리
