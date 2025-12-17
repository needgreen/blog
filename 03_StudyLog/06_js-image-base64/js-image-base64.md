# **FileReader와 Promise를 활용한 이미지 데이터(Base64) 변환 정리**

### 💡 **비동기 파일 읽기: 콜백에서 async/await로 개선하기**

- 강의 등록 기능을 구현하면서 **썸네일 이미지 업로드** 기능을 추가했다.
  사용자가 선택한 이미지 파일을 **텍스트 형태(Base64)** 로 변환해
  `<img>` 태그에 바로 표시하는 방식이다.
  이 과정에서 `FileReader`의 **비동기 처리**를 좀 더 깔끔하게 다루기 위해
  **Promise + async/await** 패턴을 적용했다.

---

### 🎨 썸네일 이미지 파일 → 문자열 변환 → `<img>` 태그 출력

```jsx
// 썸네일 파일을 dataURL(Base64)로 읽기
function readFileAsDataURL(file) {
  // 사용자가 선택한 파일
  return new Promise((resolve, reject) => {
    // 비동기 작업 관리 객체
    const fr = new FileReader(); // 파일을 자바스크립트로 읽을 수 있는 객체 생성

    fr.onload = () => resolve(fr.result); // 파일 읽기 성공 시 프로미스 성공(resolve)
    fr.onerror = reject; // 오류 발생 시 실패(reject)

    fr.readAsDataURL(file); // 파일 읽기 시작
  });
}
```

### ⚙️ 실행 순서

```jsx
fr.readAsDataURL(file);
```

- 이 줄이 실행되면 파일 읽기 **시작**
- 읽는 동안 다른 코드가 **동시에 실행됨 (비동기)**
- 파일 읽기가 끝나면 `onload` 또는 `onerror` 이벤트 발생

### 🧭 전체 흐름도

```jsx
┌─────────────────────────────────────────┐
│ readFileAsDataURL(file) 함수 호출         │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ Promise 생성 (약속 시작)                   │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ FileReader 객체 생성 (fr)                 │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ 이벤트 핸들러 등록                           │
│ - onload: 성공 시 resolve 호출             │
│ - onerror: 실패 시 reject 호출             │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ fr.readAsDataURL(file) - 읽기 시작        │
└──────────────┬──────────────────────────┘
               │
        ┌──────┴──────┐
        │             │
        ▼             ▼
    ┌───────┐    ┌────────┐
    │ 성공   │    │  실패    │
    └───┬───┘    └───┬────┘
        │            │
        ▼            ▼
   resolve(결과)  reject(에러)
        │            │
        └─────┬──────┘
              │
              ▼
        await 결과 반환

```

### 📸 읽은 파일을 Base64로 변환 (비동기 예시)

```jsx
let thumbnailUrl = '';
try {
  thumbnailUrl = await readFileAsDataURL(thumbnailFile);
} catch (error) {
  console.error(error);
  thumbnailUrl = '';
}
```

## ✅ Promise (약속 객체) 사용 이유

1. **`FileReader`는 비동기 처리**
   - 콜백 방식(`fr.onload = function() {...}`)으로 작성하면 코드 중첩이 심해짐.
   - Promise를 사용하면 결과를 `resolve`, 에러를 `reject`로 관리할 수 있다.
2. **`async/await`로 동기 코드처럼 작성 가능**

   ```jsx
   const dataURL = await readFileAsDataURL(file);
   console.log('변환 완료:', dataURL);
   ```

3. **`try...catch`로 에러 처리**
   - 실패 시 `reject`된 에러를 `catch` 블록에서 쉽게 처리할 수 있다.
   - 코드 흐름이 깔끔하고 예외 관리가 명확하다.

### 🧩 콜백 방식과 비교

```jsx
// 콜백 기반 예시 (가독성 낮음)
function readFile(file, successCallback, errorCallback) {
  const fr = new FileReader();
  fr.onload = () => successCallback(fr.result);
  fr.onerror = errorCallback;
  fr.readAsDataURL(file);
}

// 사용
readFile(
  myFile,
  (result) => {
    console.log(result);
  },
  (error) => {
    console.error(error);
  }
);
```

### 🧠 변환 데이터 이해하기

- **Base64:** 이진 데이터를 텍스트 문자열로 인코딩하는 방식
- **`readAsDataURL`:** 파일을 Base64 형태의 문자열로 변환
- **결과 문자열:** 브라우저에서 바로 `<img>` 태그에 사용 가능

```jsx
<img src="data:image/png;base64,iVBORw0KGgoAAAA...">
```

- 파일 자체를 문자열로 변환해 서버 업로드 없이 미리보기 가능
- 로컬스토리지에 이미지 저장 시에도 활용 가능

### 🔍 변환 예시

```jsx
원본 파일: cat.jpg (50KB)
      ↓ readAsDataURL
Base64: "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAA..." (약 67KB 문자열)

```

## 🧭 마무리하며

강의 등록 기능을 구현하면서 **비동기 처리의 가독성과 유지보수성**이 얼마나 중요한지 느꼈다.

`FileReader`처럼 비동기 작업이 포함된 로직은 **Promise → async/await → try/catch** 구조로 작성하면 훨씬 깔끔해진다. 이 패턴은 이미지 업로드뿐 아니라 **파일 처리 / API 통신 / 데이터 변환 등** 다양한 비동기 작업에서도 활용할 수 있다.
