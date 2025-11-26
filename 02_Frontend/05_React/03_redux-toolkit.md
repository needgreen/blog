## ✅ Redux

리액트에서 상태(state)를 효율적으로 관리하기 위한 라이브러리

- 단일 진실 원천
- 읽기 전용 상태
- 순수 함수 기반 상태 변경
- 대규모 애플리케이션에서 복잡한 컴포넌트 간 상태 공유 문제 해결

## Redux Toolkit

리덕스의 복잡한 설정, 액션, 리듀서, 스토어 코드를 짧고 간결하게 만들어주는 공식 툴세트

- configureStore, createSlice, createAsyncThunk 등 쉬운 API를 제공
- 설치

```bash
npm install @reduxjs/toolkit react-redux
```

### configureStore()

Redux 상태 관리의 중심 저장소(store)를 만드는 함수

앱 전체의 상태(state)와 reducer를 연결하고 Redux DevTools나 middleware(예: thunk)도 기본으로 설정됨

- 기본 사용 방법

  ```jsx
  import { configureStore } from '@reduxjs/toolkit';
  import counterReducer from './counterSlice'; // 슬라이스 리듀서 가져오기

  const store = configureStore({
    reducer: {
      counter: counterReducer, // 여러 슬라이스 리듀서 등록 가능
    },
  });

  export default store;
  ```

### createSlice()

액션 + 리듀서를 한 번에 만드는 함수. 상태와 관련된 로직을 한 파일에 깔끔하게 묶어주는 기능

- **state(초기값)**
- **reducers(상태 변경 함수)**
- **action creators (액션 생성 함수)**

기본 문법

```jsx
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter', // slice의 이름 (액션 타입에 prefix로 사용됨)
  initialState: { value: 0 }, // 상태의 초기값
  reducers: {
    increment: (state) => {
      state.value += 1; // immer 덕분에 직접 변경 가능
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload; // 액션에 전달된 데이터 사용
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

- 주요 옵션

  | 옵션 이름         | 설명                                                                       |
  | ----------------- | -------------------------------------------------------------------------- |
  | **name**          | 슬라이스 이름. 자동으로 액션 타입 앞에 prefix로 붙음 (`counter/increment`) |
  | **initialState**  | 상태의 초기값 정의                                                         |
  | **reducers**      | 상태를 변경하는 함수들(=액션 처리 로직)                                    |
  | **extraReducers** | 다른 slice나 asyncThunk에서 발생한 액션 처리용                             |

- 스토어와 연결 예시

  ```jsx
  import { configureStore } from '@reduxjs/toolkit';
  import counterReducer from './counterSlice';

  export const store = configureStore({
    reducer: {
      counter: counterReducer,
    },
  });
  ```

  컴포넌트에서 사용 예시

  ```jsx
  import React from 'react';
  import { useSelector, useDispatch } from 'react-redux';
  import { increment, decrement } from './counterSlice';

  function Counter() {
    const count = useSelector((state) => state.counter.value);
    const dispatch = useDispatch();

    return (
      <>
        <h1>{count}</h1>
        <button onClick={() => dispatch(increment())}>+1</button>
        <button onClick={() => dispatch(decrement())}>-1</button>
      </>
    );
  }

  export default Counter;
  ```

### createAsyncThunk()

비동기 작업(API 요청 등)을 깔끔하게 관리할 수 있게 해주는 함수

- 기본 문법

  ```jsx
  import { createAsyncThunk } from '@reduxjs/toolkit';

  export const fetchUser = createAsyncThunk(
    'user/fetchUser', // 액션 타입
    async (userId, thunkAPI) => {
      const response = await fetch(`https://api.example.com/users/${userId}`);
      return await response.json(); // 이 데이터가 fulfilled 시 payload로 전달됨
    }
  );
  ```

- 자동 생성되는 액션 타입
  | 액션 타입 | 의미 |
  | -------------------------- | ---------------------------- |
  | `user/fetchUser/pending` | 비동기 요청 시작 |
  | `user/fetchUser/fulfilled` | 요청 성공 (데이터 수신 완료) |
  | `user/fetchUser/rejected` | 요청 실패 (에러 발생) |

| 속성                       | 설명                                    |
| -------------------------- | --------------------------------------- |
| **dispatch**               | 다른 액션을 디스패치할 수 있음          |
| **getState**               | 현재 스토어 상태에 접근                 |
| **rejectWithValue(error)** | 에러를 커스텀 형태로 반환               |
| **signal**                 | AbortController를 이용한 요청 취소 제어 |
| **extra**                  | 미들웨어 설정 시 외부 데이터 전달 가능  |
