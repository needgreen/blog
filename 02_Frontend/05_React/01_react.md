## React

- React 라이브러리 : https://ko.legacy.reactjs.org/docs/cdn-links.html
  ```jsx
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  ```
- React에서 만든 UI를 실제 브라우저의 DOM에 렌더링 할 수 있게 해주는 라이브러리
- React의 가상 DOM(Virtual DOM)과 실제 DOM 간의 연결고리 역할을 하며, React Element를 브라우저 화면에 그려줍니다.
- React 18부터는 `ReactDOM.createRoot`와 같은 새로운 API를 통해 더 효율적이고 동시적인 렌더링(concurrent rendering)을 지원
- 실제 배포시에는 production 버전을 사용하는 것이 권장

## Element

- 리액트 애플리케이션을 구성하는 가장 작은 단위
- 화면세 표시되는 내용을 기술하는 자바스크립트 객체, 실제 **DOM 요소의 가상 표현**
- React Element는 불변하며 한 번 생성되면 변경되지 않음.
- `React.createElement()`

## Rendering

- React Element 같은 구성 요소들을 화면에 그리는 작업
- Root DOM Node: ReactDOM에서 관리하는 DOM Node
- `ReactDOM`은 Element와 그 자식 Element를 이전의 Element와 비교하고 변경된 부분만 업데이트 함

## JSX

- babel.min.js : https://babeljs.io/setup#installation
  ```jsx
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  ```
- 바벨의 브라우저용 독립 실행 파일을 불러오는 역할
- `<script type="text/babel">` 작성
- JSX(JavaScript XML)는 React에서 UI를 정의하기 위해 사용하는 자바스크립트의 확장 문법
- HTML과 매우 유사한 구조를 자바스크립트 코드 안에서 직접 사용할 수 있게 해주며, 가독성과 직관성을 높여줍니다.
- 단일 루트 요소
  - JSX에서 여러 태그를 반환할 때 반드시 하나의 부모(루트) 요소로 감싸야 합니다.
  - 사용할 태그가 없는 경우에는 React가 지원하는 Fragment를 사용할 수 있습니다. (`<React.Fragment></React.Fragment>` 또는 `<></>`)
- 자바스크립트 표현식 삽입 : 내부에 { } 중괄호 사용으로 변수, 연산, 함수 호출, 주석 등을 삽입
- 속성
  - class -> `className`
  - for -> `htmlFor`
  - onclick -> `onClick` (이벤트 속성: 기본 소문자 조합에서 camelCase 방식으로 변경)
- 닫는 태그 필수 `<br />` `<img />` `<input />`
- 조건부 렌더링
  - 삼항 연산자, 논리 연산자(&&) 등 사용 조건에 따라 다른 내용 렌더링 가능
- 스타일 적용
  - CSS 속성명 카멜케이스 방식으로 변경해 사용. 예) `style={{backgroudColor: ‘black’, ...}}`

## Component

- 컴포넌트는 특정 기능이나 UI를 담당하는 독립적인 코드 블록
- 여러 개의 Element를 조합, 재사용이 가능
- props와 state를 통해 동적인 데이터를 다루고, 상태(state)가 변경될 때마다 자동으로 다시 렌더링
- 컴포넌트의 이름은 PascalCase 방식
- 리액트에서는 버튼, 폼 등 대부분의 구성 요소를 컴포넌트로 만들어서 사용
- 클래스형 컴포넌트
  - React.Component를 상속 받아서 작성
  - `render() { return 엘리먼트 }` 형식으로 화면에 표시할 엘리먼트를 반환
  - 예) `class Welcome extends React.Component { ... }`
- 함수형 컴포넌트 : 함수선언식, 함수표현식 모두 가능
  - 예) `function Welcome(props) { ... }` `const FunctionComp = () => { ... }`

## Props(properies)

- props는 리액트에서 컴포넌트 간에 데이터를 전달할 때 사용하는 객체
- 부모 컴포넌트가 자식 컴포넌트에게 데이터나 설정값을 넘겨줄 때 활용
- 컴포넌트의 동작과 렌더링 결과를 동적으로 제어할 수 있다.
- 데이터 전달
  - props는 상위(부모) 컴포넌트에서 하위(자식) 컴포넌트로 데이터를 전달하는 역할
  - JSX에서 HTML 속성처럼 사용하며, 자식 컴포넌트에서는 함수의 매개변수(파라미터)로 전달받아 사용
- 읽기 전용(불변성)
  - props는 컴포넌트 내부에서 변경할 수 없는 읽기 전용 속성
  - 자식 컴포넌트는 props의 값을 직접 수정할 수 없고 오직 부모 컴포넌트에서만 값을 바꿀 수 있다.
- 컴포넌트 재사용성 향상
  - 하나의 컴포넌트를 다양한 데이터와 설정으로 재사용 할 수 있다.
  - 같은 버튼 컴포넌트에 다른 텍스트, 색상, 동작 등을 props로 넘겨 다양하게 활용 가능
- 동적 렌더링과 자동 업데이트
  - 부모 컴포넌트에서 전달한 props 값이 변경되면, 해당 props를 사용하는 자식 컴포넌트는 자동으로 리렌더링되며 동적인 UI 구현이 가능
- 함수형 컴포넌트에서는 매개변수로 접근
  - 예:
  ```jsx
  //----- ① 함수형 컴포넌트
  function ChildComponent(props) {
    return <h1>{props.greeting}</h1>;
  }
  ```
- 클래스형 컴포넌트에서는 `this.props`로 접근

  - 예 :

  ```
  //----- ② 클래스형 컴포넌트
   class ChildComponent extends React.Component {
     render() {
       return <h1>{this.props.greeting}</h1>;
     }
   }
  ```

- props 객체를 객체 구조 분해 할당으로 간결하게 사용
  ```jsx
  예시)
          function ChildComponent({ color, name }) {
            return <div style={{ color }}>{ name }</div>;
          }
  ```
- `defaultProps`를 이용해 props가 전달되지 않았을 때 사용할 기본값을 설정

  ```jsx
  예시)
            ChildComponent.defaultProps = {
              color: "green",
              name: "이름 없음",
            };
          * default parameter 이용해서 기본값 설정 가능(권장)
  ```

- 컴포넌트 태그 사이에 들어가는 내용을 `props.children`으로 접근

  ```jsx
  예시)
          function ChildComponent({ children }) {
            return <div>{ children }</div>;
          }
  ```

- props drilling
  - 여러 단계의 중간 컴포넌트를 거쳐 props를 전달하는 현상으로 컴포넌트 구조가 복잡해질 수 있음.
  - Context API, Redux, Recoil 등의 상태 관리 도구를 사용하여 해결 가능

## Event

- 웹 브라우저에서 DOM 요소와 상호작용할 때 발생하는 사건(클릭, 입력, 마우스 이동 등)을 의미
- 이벤트 속성 이름 표기법
  - HTML에서는 소문자(`onclick`, `onchange`)를 사용
  - 리액트에서는 camelCase 방식(`onClick`, `onChange`)으로 작성
- 이벤트 핸들러 전달 방식
  - 함수를 중괄호(`{}`)로 전달
  - 함수명 뒤에 괄호를 붙이지 않고 함수 참조만 전달 (괄호를 붙이면 즉시 실행됨)
- 이벤트 객체
  - 이벤트 발생 시 이벤트 핸들러 함수의 첫 번째 인자로 이벤트 객체가 전달
  - 자식요소의 이벤트가 상위 요소로 전파되는 이벤트전파(이벤트 버블링)을 막고자할 경우 `stopPropagation()` 메소드를 사용

## State

- state는 컴포넌트 내부에서 관리되는 동적인 데이터를 의미
- state가 변경되면 해당 컴포넌트와 그 자식 컴포넌트가 자동으로 다시 렌더링
- 클래스형 컴포넌트는 state를 직접 사용할 수 있다.
- 함수형 컴포넌트는 v16.8 이후 `React.useState()` 훅(Hooks)을 이용해서 state를 사용할 수 있다.
- state 선언 및 초기화
  - 클래스형 컴포넌트에서는 state를 객체 형태로 선언. 저장하는 값은 객체 리터럴 형식 {}
  - 생성자(`constructor`)에서 `this.state`로 초기화
  - ES6 문법을 사용하면 생성자 없이 클래스 필드로 바로 선언 가능
- state 값 변경
  - 반드시 `this.setState()` 메소드를 사용
    - `this.state`를 직접 변경하면 렌더링이 발생되지 않음
  - `setState()`
    - 호출하면 리액트가 변경을 감지하고 컴포넌트를 리렌더링
    - setState() 메소드는 비동기 방식으로 백그라운드에서 동작
    - 하나의 이벤트에서 `setState()` 메소드를 여러 번 호출하더라도 state 값이 순차적으로 사용되지 않음
  - prevState

# React

### **권장 워크플로우**

1. **신규 프로젝트**: Vite 사용 권장 ⚡
2. **기존 CRA 프로젝트**: 유지보수 가능하나 점진적 마이그레이션 고려
3. **대규모 프로젝트**: Next.js 같은 풀스택 프레임워크 검토

**📊 CRA vs Vite 비교**

| 항목           | Create React App | Vite      |
| -------------- | ---------------- | --------- |
| 개발 서버 속도 | 느림             | 매우 빠름 |
| 빌드 속도      | 보통             | 빠름      |
| 설정 복잡도    | 간단             | 보통      |
| 번들 크기      | 큼               | 작음      |
| 안정성         | 높음             | 보        |

### **Vite를 이용한 React 앱 만들기**

```jsx
# 1. 프로젝트 만들기
npm create vite@latest [프로젝트명] -- --template react
# Use rolldown-vite? : No
# Install with npm and start now? : Yes

# 2. 프로젝트 폴더로 이동
cd [프로젝트경로]

# 3. 의존성 설치 (위에 Install with npm and start now를 Yes할 경우 생략 가능)
npm install

# 4. 프로젝트 실행 (개발 서버 실행)
npm run dev

# 5. 프로젝트 종료 (개발 서버 종료)
# ctrl + c (키 입력)
```

**React 앱 구조 (Vite 기준)**

```
[프로젝트명]/
 ├── public/                    # 정적 파일들
 ├── src/                       # 소스 코드
 │   ├── assets/                # 이미지, 폰트 등 리소스
 │   ├── App.jsx                # 메인 App 컴포넌트
 │   ├── App.css                # App 컴포넌트 스타일
 │   ├── index.css              # 전역 스타일
 │   └── main.jsx               # 앱 진입점
 ├── eslint.config.js           # ESLint 설정
 ├── index.html                 # Vite 진입점 HTML (루트에 위치)
 ├── package.json               # 프로젝트 설정 및 의존성
 ├── package-lock.json          # 의존성 잠금 파일
 └── vite.config.js             # Vite 설정 파일
```

**주요 npm 명령어**

```bash
npm start# 개발 서버 시작 (CRA)
npm run dev# 개발 서버 시작 (Vite)
npm run build# 프로덕션 빌드
npm test# 테스트 실행
npm run preview# 빌드 결과 미리보기 (Vite)
```

- .gitignore ⇒ 깃에 업로드 하지 않을 파일 모음
- eslint.config.js ⇒ 코드 규칙 설정 파일
- vite 에서 jsx 문법 사용 시 .jsx 확장자로 저장해야 읽을 수 있음.

## **git 전송**

- gitignore 파일 먼저 진행 `git add 경로/.gitignore`
  - node_modules 제외하고 업로드 해야 함
- git add . / git commit -m ‘message’ / git push -u origin main

# React Vscode 단축키

- rfc : 함수형 컴포넌트 단축키
- rfce : 함수형 컴포넌트+export 단축키
