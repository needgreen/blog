# GraphQL 정리

## 1. GraphQL 개요

### GraphQL이란?

API를 위한 쿼리 언어이자 서버 측 런타임. 클라이언트가 필요한 데이터의 구조를 정확하게 정의하고 요청할 수 있다.

### REST API의 문제점과 GraphQL의 해결

- **Over-fetching**: REST는 엔드포인트에서 모든 데이터를 반환. `name`만 필요해도 `id`, `email`, `age` 등 불필요한 데이터까지 받아와야 한다. GraphQL은 필요한 필드만 명시하면 해당 데이터만 반환.
- **Under-fetching**: REST에서 연관 데이터를 가져오려면 여러 번 요청 필요. 게시글 + 작성자 + 댓글을 가져오려면 3번의 API 호출이 필요하다. GraphQL은 한 번의 쿼리로 중첩된 모든 데이터를 요청 가능.
- **강력한 타입 시스템**: 스키마 정의 언어(SDL)로 API 데이터 구조를 명확하게 정의. 서버와 클라이언트 간의 계약 역할.

### 3가지 핵심 작업

- **Query**: 데이터 읽기 (GET)
- **Mutation**: 데이터 생성/수정/삭제 (POST, PUT, DELETE)
- **Subscription**: 실시간 데이터 구독

## 2. 서버 구축 (Apollo Server)

### 라이브러리 설치

```bash
// 프로젝트 시작
npm init -y

// 라이브러리 설치
npm install @apollo/server @as-integrations/express5 express graphql cors

```

### 기본 구조 `index.js`

```jsx
// 필요한 모듈들
import { ApolloServer } from '@apollo/server'; // Apollo Server 핵심 라이브러리
import { expressMiddleware } from '@as-integrations/express5'; // Express에 Apollo Server를 연결해 주는 미들웨어
import express from 'express'; // HTTP 서버(Express) 모듈
import cors from 'cors'; // CORS 허용을 위한 모듈

// 1. GraphQL 스키마 정의(Type Definitions): 어떤 타입과 쿼리를 제공할지 선언합니다.
//    - API의 데이터 구조를 정의하는 과정 (메뉴판)
//    - Query 타입은 모든 GraphQL 스키마에 필수이며, 데이터 조회(읽기)의 진입점
const typeDefs = `
  type Query {
    hello: String
    user: User
  }

  type User {
    id: ID!
    username: String
  }
`;

// 2. 리졸버: 스키마에 정의된 필드가 실제로 어떤 데이터를 반환할지 구현합니다.
//    - 스키마 정의에 따라 실제 데이터를 반환하는 함수 (요리법)
const resolvers = {
  Query: {
    // 2_1) hello 쿼리가 요청되면 "Hello, GraphQL World!" 문자열을 반환합니다.
    hello: () => 'Hello, GraphQL World!',
    // 2_2) user 쿼리가 요청되면 객체를 반환합니다.
    user: () => {
      return {
        id: 'user-001', // - user 쿼리의 id 필드가 요청되면 "user-001" 문자열을 반환합니다.
        username: 'GraphQLStudent', // - user 쿼리의 username 필드가 요청되면 "GraphQLStudent" 문자열을 반환합니다.
      };
    },
  },
};

// 3. Apollo Server와 Express를 초기화하고 실행하는 함수
async function startServer() {
  // 3_1) GraphQL 요청을 처리할 Apollo Server 인스턴스 생성 (typeDefs와 resolvers를 사용하여 생성)
  const server = new ApolloServer({
    typeDefs,
    resolvers,
  });
  // 3_2) Apollo Server가 요청을 받을 준비를 마칠 때까지 대기
  await server.start();

  // 3_3) REST 엔드포인트, 미들웨어 등을 연결할 Express 앱 생성
  const app = express();
  // 3_4) /graphql 엔드포인트로 들어오는 요청에 대한 미들웨어 체인 구성
  app.use(
    '/graphql', // - /graphql 엔드포인트로 들어오는 요청에 대한 미들웨어 체인 구성
    cors(), // - 다른 도메인에서 접근할 수 있도록 CORS 허용
    express.json(), // - 요청 본문을 JSON으로 파싱
    expressMiddleware(server) // - Apollo Server와 Express를 연결 (Express 에서 GraphQL 요청을 처리할 수 있도록 연결)
  );
  // 3_5) 서버가 열릴 포트 지정 후 실행
  const PORT = 4000;
  app.listen(PORT, () => {
    console.log(`🚀 GraphQL 서버가 http://localhost:${PORT}/graphql 에서 실행 중입니다.`);
  });
}

// 4. 서버 실행
startServer();
```

**JSON 파일 수정 `package.json`**

```json
{
  "name": "graphql-server-example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module", // <-- 이 라인을 추가하세요!
  "scripts": {
    "start": "node index.js" // <-- 이 라인을 추가하세요!
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@apollo/server": "^5.x.x", // 버전은 다를 수 있습니다.
    "@as-integrations/express5": "^1.x.x",
    "cors": "^2.x.x",
    "express": "^5.x.x",
    "graphql": "^16.x.x"
  }
}
```

**서버 실행 (터미널)**

```bash
npm start
```

**Apollo Sandbox 접속**

웹 브라우저에서 `http://localhost:4000/graphql` 접속

## 3. Query 문법

### 기본 필드 요청

```graphql
query {
  users {
    username
  }
}
```

### 중첩 객체

```graphql
query {
  posts {
    title
    author {
      username
    }
  }
}
```

### 인수 (Arguments)

```graphql
query {
  user(id: "1") {
    username
    age
  }
}
```

### 별칭 (Aliases)

동일한 필드를 다른 인수로 여러 번 호출할 때 사용.

```graphql
query {
  userOne: user(id: "1") {
    username
  }
  userTwo: user(id: "2") {
    username
  }
}
```

### 프래그먼트 (Fragments)

반복되는 필드 집합을 재사용.

```graphql
query {
  userOne: user(id: "1") {
    ...UserInfo
  }
  userTwo: user(id: "2") {
    ...UserInfo
  }
}

fragment UserInfo on User {
  id
  username
  age
}
```

### 변수 (Variables)

동적 값을 쿼리에 전달. 보안상 문자열 조합보다 안전하다.

```graphql
query GetUserByVariable($uid: ID!) {
  user(id: $uid) {
    id
    username
  }
}
```

Variables 패널:

```json
{ "uid": "2" }
```

## 4. Mutation 문법

### 기본 사용

```graphql
mutation CreateNewPost($title: String!, $authorId: ID!) {
  createPost(title: $title, authorId: $authorId) {
    id
    title
    author {
      username
    }
  }
}
```

작업 완료 후 반환받을 데이터 구조를 직접 지정 가능

## 5. React에서 사용하기 (Apollo Client)

### 설치 및 설정

```bash
npm install @apollo/client graphql

```

```jsx
// main.jsx
import { ApolloClient, InMemoryCache, HttpLink } from '@apollo/client/core';
import { ApolloProvider } from '@apollo/client/react';

const client = new ApolloClient({
  link: new HttpLink({ uri: 'http://localhost:4000/graphql' }),
  cache: new InMemoryCache(),
});

<ApolloProvider client={client}>
  <App />
</ApolloProvider>;
```

### useQuery (데이터 읽기)

```jsx
import { gql, useQuery } from '@apollo/client';

const GET_ALL_USERS = gql`
  query {
    users {
      id
      username
    }
  }
`;

function UserList() {
  const { loading, error, data } = useQuery(GET_ALL_USERS);

  if (loading) return <p>로딩 중...</p>;
  if (error) return <p>에러: {error.message}</p>;

  return data.users.map((user) => <li key={user.id}>{user.username}</li>);
}
```

### useQuery + Variables

```jsx
const { data } = useQuery(GET_USER_BY_ID, {
  variables: { uid: userId },
});
```

### useMutation (데이터 쓰기)

```jsx
const [createPost, { loading, error }] = useMutation(CREATE_POST_MUTATION, {
  refetchQueries: [GET_ALL_POSTS], // 성공 시 목록 갱신
});

const handleSubmit = () => {
  createPost({ variables: { title, authorId } });
};
```

`useQuery`는 호출 시 즉시 실행되지만, `useMutation`은 반환된 함수를 호출해야 실행된다.

- 상태 관리는 ui로만 사용 , 로직에는 사용하지 않음
