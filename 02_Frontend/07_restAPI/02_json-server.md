## json-server를 활용한 Mock API 구축

json-server는 JSON 파일 하나로 완벽한 가짜 REST API 서버를 구축할 수 있는 도구이다.

### 설치 및 설정

```bash
npm install -g json-server

```

### json-server 설치 및 실행

```bash
npx json-server --watch db.json
```

### db.json 파일 생성

```json
{
  "posts": [
    {
      "id": 1,
      "title": "json-server 소개",
      "author": "dev_senior"
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "정말 유용하네요!",
      "postId": 1
    }
  ]
}
```

최상위는 객체로 감싸고, 각 Key가 API 엔드포인트가 된다.

### 서버 실행

```bash
# --watch: db.json 파일 변경을 감지하여 자동 새로고침
# --port 3001: 서버 포트 지정 (Vite의 5173과 겹치지 않게)
json-server --watch db.json --port 3001
```

이렇게 실행하면 **자동으로 REST API 엔드포인트가 생성**된다.

- `GET /posts`: 모든 게시글 조회
- `GET /posts/1`: 특정 게시글 조회
- `POST /posts`: 새 게시글 생성
- `PATCH /posts/1`: 게시글 일부 수정
- `DELETE /posts/1`: 게시글 삭제

## React에서 API 호출하기

컴포넌트가 처음 렌더링될 때 `useEffect` 훅을 사용하여 데이터를 가져온다.

의존성 배열을 `[]`로 설정하면 한 번만 실행된다.

### fetch API 사용

fetch는 브라우저 내장 API로 별도 설치가 필요 없다. 하지만 응답을 `.json()`으로 변환하는 과정이 필요하다.

```jsx
useEffect(() => {
  fetch('http://localhost:3001/posts')
    .then((response) => {
      if (!response.ok) {
        throw new Error('데이터를 불러오는 데 실패했습니다.');
      }
      return response.json();
    })
    .then((data) => {
      setPosts(data);
      setLoading(false);
    })
    .catch((error) => {
      setError(error.message);
      setLoading(false);
    });
}, []);
```

### axios 사용

axios는 설치가 필요하지만 더 편리하게 API 통신을 처리할 수 있다.

```bash
npm install axios

```

**axios의 장점**:

- JSON 변환이 자동으로 처리된다
- 메서드를 명확하게 구분하여 사용할 수 있다
- 에러 핸들링이 더 직관적이다

```jsx
useEffect(() => {
  axios
    .get('http://localhost:3001/posts')
    .then((response) => {
      setPosts(response.data);
      setLoading(false);
    })
    .catch((error) => {
      setError(error.message);
      setLoading(false);
    });
}, []);
```

### POST 요청 예시 (axios)

```jsx
const createPost = () => {
  const newPost = {
    title: 'Axios로 만든 새 글',
    author: 'axios_user',
  };

  axios
    .post('http://localhost:3001/posts', newPost)
    .then((response) => {
      setPosts((prevPosts) => [...prevPosts, response.data]);
    })
    .catch((error) => console.error('Error:', error));
};
```

## 주요 HTTP 상태 코드 정리

### ✅ **2xx: 성공 (Success)**

| 상태 코드          | 이름          | 설명                                                    |
| ------------------ | ------------- | ------------------------------------------------------- |
| **200 OK**         | 요청 성공     | 요청이 정상적으로 처리됨                                |
| **201 Created**    | 리소스 생성됨 | 요청 성공 + 새로운 리소스가 생성됨 (POST에 주로 사용)   |
| **204 No Content** | 내용 없음     | 요청 성공했지만 응답 데이터는 없음 (DELETE에 주로 사용) |

### ⚠️ **4xx: 클라이언트 오류 (Client Error)**

| 상태 코드            | 이름        | 설명                                                 |
| -------------------- | ----------- | ---------------------------------------------------- |
| **400 Bad Request**  | 잘못된 요청 | 문법 오류 또는 유효하지 않은 데이터로 인해 처리 불가 |
| **401 Unauthorized** | 인증 필요   | 로그인이 필요하며 인증이 되어 있지 않음              |
| **403 Forbidden**    | 접근 금지   | 요청을 이해했지만 권한 부족으로 거부됨               |
| **404 Not Found**    | 리소스 없음 | 요청한 URL 또는 리소스를 찾을 수 없음                |

### 🔥 **5xx: 서버 오류 (Server Error)**

| 상태 코드                     | 이름              | 설명                                                         |
| ----------------------------- | ----------------- | ------------------------------------------------------------ |
| **500 Internal Server Error** | 서버 오류         | 서버 내부 문제로 요청을 처리하지 못함                        |
| **502 Bad Gateway**           | 잘못된 게이트웨이 | 게이트웨이/프록시 서버가 유효하지 않은 응답을 받음           |
| **503 Service Unavailable**   | 서비스 불가       | 서버가 과부하 또는 유지보수로 인해 일시적으로 응답할 수 없음 |

## 학습 마무리

Mock API는 프론트엔드 개발의 생산성을 크게 향상시킨다. json-server를 활용하면 간단한 JSON 파일만으로 완전한 REST API를 구축할 수 있으며, fetch나 axios를 통해 React에서 손쉽게 데이터를 연동할 수 있다. 실무에서는 axios가 더 직관적이고 편리한 API를 제공하여 많이 사용된다.
