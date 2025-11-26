## TypeScript 란?

- JavaScript 기능 + 정적 타입 시스템을 추가한 언어
- 마이크로소프트가 개발한 오픈소스
- 모든 JavaScript 코드는 TypeScript에서도 유효
- `.ts`, `.tsx` 파일은 브라우저애소 바로 실행되지 않음 : 컴파일 JS로 변환 과정 필요

### 타입 스크립트 사용 이유

- JavaScript는 동적 타입 언어로 코드가 실행되는 시점인 런타임에 타입이 결정됨.
- 코드가 복잡해질수록 타입 관련 오류 발생 가능

1. 조기 에러 발견
   1. JS(런타임 에러) : 실행 중 타입 오류 발생
   2. TS(컴파일 에러) : 코드를 작성하는 순간 타입 에러 알려줌 → 버그 사전 차단
2. 코드 품질 및 유지보수 향상
   1. 타입 명시로 코드 의도가 명확해짐
   2. 리팩토링 시 타입 시스템이 변경 영향을 자동으로 추적
3. 강력한 개발자 경험
   1. 정확하고 유용한 자동 완성 및 타입 기반 코드 제안
   2. 대규모 프로젝트에서 협업 시 API, 함수 등의 사용이 안전해지고 예측 가능함

### 타입 스크립트 개발 환경 설정

1. 설치

   - 글로벌 설치(시스템 전체)
     ```bash
     npm install -g typescript
     ```
   - 프로젝트 로컬 설치
     ```bash
     npm install typescript --save-dev
     ```
   - 설치 후 터미널에서 확인
     ```bash
     tsc -v
     ```

1. tsconfig.json (TS 설정 파일)

   - 프로젝트 루트에 위치하며 TS 컴파일 옵션을 정의
   - 생성:

   ```bash
   tsc --init
   ```

   **주요 옵션**

   - `target`: 컴파일 후 JS 버전 (예: ES6, ES2020)
   - `module`: 모듈 방식 설정 (예: CommonJS, ESNext)
   - `strict`: 엄격한 타입 검사 활성화
   - `jsx`: React JSX 처리를 위한 설정
   - `outDir`: 컴파일된 JS 파일 위치
   - `rootDir`: TS 소스 경로

1. Vite + React 로 TypeScript 프로젝트 시작하기
   - **프로젝트 생성**

```bash
npm create vite@latest my-react-ts-app -- --template react-ts
```

- **자동 구성**
  - TS 설정이 자동 구성된 `tsconfig.json` 생성
  - `.tsx` 기반 파일 구조 제공 (`main.tsx`, `App.tsx`)
  - Vite 설정도 TypeScript로 작성됨(`vite.config.ts`)
- **실행**

```bash
cd my-react-ts-app
npm install
npm run dev
```

## TypeScript 기본 타입

### 타입 주석

TypeScript에서는 변수, 매개변수, 반환 값 뒤에 `:`와 타입을 붙여 **어떤 타입만 가질 수 있는지 명확하게 선언**할 수 있다.

```tsx
let name: string = 'Steve';
let age: number = 30;
let isStudent: boolean = false;
```

**왜 필요할까?**

- 의도를 명확히 표현해 변수의 용도를 분명하게 전달함
- 버그를 사전에 차단하여 런타임 오류를 줄임

**JavaScript 기본 원시 타입(TypeScript에서의 사용)**

```tsx
// string
let message: string = 'Hello, TypeScript!';

//number
//정수, 실수, `NaN`, `Infinity` 모두 포함
let count: number = 100;

// boolean
let isLoading: boolean = true;

// null / undefined
// `strictNullChecks: true`일 경우 일반 타입에 직접 할당 불가
// 필요 시 Union Type 사용

let nullableName: string | null = 'Steve';
nullableName = null; // OK
```

### 특수 타입

- **any (가능한 사용 자제)**
  모든 타입을 허용하며, 타입 검사를 비활성화함.
  마이그레이션 과정 등 불가피한 상황에만 사용.

```tsx
let anything: any = 10;
anything = 'Hello';
```

- **unknown (any보다 안전한 선택)**
  어떤 값이든 받을 수 있지만, **사용 전 타입 체크 필수**.

```tsx
let maybe: unknown = 10;

if (typeof maybe === 'number') {
  let num: number = maybe; // OK
}
```

### 배열(Array)

- 두 가지 방식으로 타입 선언 가능.

**1) 타입[] (더 간결해서 권장)**

```tsx
let numbers: number[] = [1, 2, 3];
```

**2) Array<타입>**

```tsx
let numbers: Array<number> = [1, 2, 3];
```

### 튜플(Tuple)

- **길이 고정**, **요소별 타입 지정된 배열**
- React `useState` 반환값처럼 구조가 정해진 데이터에 유용

```tsx
let person: [string, number] = ['Steve', 30];
```

### 이넘(Enum)

- 관련 있는 상수값들의 집합에 이름을 붙여 관리하는 기능
- 권한, 상태, 방향 등 제한된 값만 사용해야 할 때 가독성 향상

```tsx
enum Color {
  RED,
  GREEN,
  BLUE,
}

let col: Color;
col = Color.RED;
col = Color.GREEN;
col = Color.BLUE;
```

### 유니언(Union)

- 타입1 | 타입2 | 타입3 : |(파이프 기호) 사용
- 여러 타입을 표현할 때 사용

```tsx
// 방향에 대한 값(up, down, left, right)
let direction: 'up' | 'down' | 'left' | 'right';
// 리터럴 타입 ""
direction = 'down';

let user: {
  name: string;
  age: number;
  role: 'ADMIN' | 'USER' | 'GUEST';
};

let product: {} | null;
```

## 객체(Object) 타입 정의

Type Alias 와 Interface로 정의

### Type Alias(타입 별칭)

- type 키워드를 사용해 새로운 타입의 별칭을 만드는 것
- 객체 모양 뿐만 아니라 원시값, 유니언 타입, 튜플 등 거의 모든 타입에 별칭 가능
- 문법 : `type TypeName = { ... };`
  - type 타입별칭 = 타입정의;

```tsx
// 1) 기본 타입 별칭
type Age = number;

let userAge: Age = 32;
userAge = 25;

// 2) 리터럴 타입 별칭
type Name = 'Kim' | 'Lee' | 'Park';
let userName: Name = 'Kim';
userName = 'Lee';

type Greeting = `Hello ${Name}`; // TypeScript 4.1 이상부터
let message: Greeting = 'Hello Kim';
message = 'Hello Lee';
// message = 'Hello Yun';

type StatusCode = 200 | 400 | 404 | 500;
let status: StatusCode = 200;

// 3) 객체 타입 별칭
type User = {
  id: number | string;
  name: Name;
  email: string;
  isAdmin: boolean;
};

let user1: User = {
  id: 1,
  name: 'Kim',
  email: 'kim@test.com',
  isAdmin: false,
};
console.log(user1);
```

- **? 옵셔널 속성**

```tsx
// 타입 정의
type Person = {
  name: string;
  age: number;
  job?: string; // ? : optional, 선택적 속성
};

let person1: Person = {
  name: '김코드',
  age: 32,
  job: 'developer',
};
```

### **Intersection Types(인터섹션 타입)**

- 타입 & 타입 & 타입

```tsx
type Person = {
  name: string;
  age: number;
  job?: string;
};

type Worker = {
  company: string;
  position: string;
};

type Employee = Person & Worker; // 두 타입의 속성을 모두 포함하는 타입 정의

let emp: Employee = {
  name: 'John',
  age: 30,
  job: 'developer',
  company: 'Google',
  position: 'CTO',
};
```

### Interface(인터페이스)

- `interface` 키워드를 사용 객체의 모양 정의
- 주로 객체의 구조 정의(설계도 또는 계약으로 사용, 클래스가 특정 구조를 따르도록 강제할 때)
- 문법 : `interface User { ... }`
  ```tsx
  interface 인터페이스명 {
    프로퍼티: 타입;
    메서드: 타입;
  }
  ```
- 사용 예시

```tsx
interface User {
  id: number;
  name: string;
  email?: string; // 선택사항일 경우 ? 사용
}

const user1: User = {
  id: 1,
  name: '김코딩',
};

// 인터페이스 확장 extends
interface Student extends User {
  // User 인터페이스의 프로퍼티 상속받음
  grade: number;
}

const stu1: Student = {
  id: 3,
  name: 'Lee code',
  grade: 2,
};

// 인터페이스 선언 병합

interface Person {
  name: string;
}

interface Person {
  age: number;
}

const per: Person = {
  name: '장코드',
  age: 22,
};
```

### **Type Alias VS Interface 차이점**

- Type Alias, Interface 모두 객체의 타입(구조)을 정의하는데 사용

1.  확장

- Type Alias : 확장 불가능, 단 Intersection(&)으로 대체 가능
- Interface : 확장 가능(extends 키워드 사용)

2.  병합

- Type Alias : 병합 불가, 동일 이름의 타입 별칭 선언 불가
- Interface : 병합 가능, 동일한 이름의 인터페이스 선언 가능

✅ **권장 사용**

- Interface 사용 권장
  - API 응답 데이터 작성, React 컴포넌트 Props 값으로 전달 받을 때 객체 구조 정의 시
  - extends를 통한 확장이 더 직관적이고 선언 병합 기능으로 유연성 제공
  - 기존 라이브러리에서 정의해둔 인터페이스에 내가 추가적인 속성을 덧붙일 때 용이
- Type Alias 사용 권장
  - 유니언 타입 정의 시, 원시값에 의미 부여할 때, 튜플 정의할 때, 복잡한 타입 정의할 때

## TypeScript 함수

### 1. 매개변수 및 반환 타입 정의

TypeScript는 함수의 입력(매개변수)과 출력(반환 값)을 명확하게 타입으로 지정할 수 있음.

```tsx
function add(x: number, y: number): number {
  return x + y;
}

const subtract = (x: number, y: number): number => {
  return x - y;
};
```

- 매개변수 타입: `(x: number)`
- 반환 타입: `: number`
- 잘못된 타입 전달 시 컴파일 단계에서 오류 발생

### 2. void 타입

아무것도 반환하지 않는 함수에 사용하는 타입.

```tsx
function printMessage(message: string): void {
  console.log(message);
}
```

- 반환값이 없거나 `undefined`를 반환하는 함수에 적용됨

### 3. 선택적 매개변수 (Optional Parameter)

- 필수가 아닌 매개변수는 이름 뒤에 `?`를 붙여 선언한다.

```tsx
function getMessage(name: string, msg?: string): string {
  return `${msg || `Hello`}, ${name}`;
}
console.log(getMessage('김유저', 'hi~!! '));
console.log(getMessage('박유저')
```

- 선택적 매개변수는 **필수 매개변수 뒤에 위치**해야 함

### 4. 기본 매개변수 (Default Parameter)

값이 전달되지 않을 경우 기본값을 사용하는 매개변수.

```tsx
function repeat(text: string, count: number = 1): string {
  return text.repeat(count);
}
```

- 기본값을 통해 타입이 자동 추론됨

### 5. 나머지 매개변수(Rest Parameter)

- 매개변수명 앞에 `...` 을 붙여 인자를 배열로 받음
- **필수 매개변수 뒤에 위치** 해야 함

```tsx
function joinString(separator: string, ...string: string[]): string {
  console.log(string.length);
  return string.join(separator);
}

console.log(joinString('-', 'Hello', 'world')); // strings === ["hello", "world"]
console.log(joinString(' ', 'Hello', 'world', '!')); // strings === ["hello", "world", "!"]
```

### 6. 함수 타입 표현식 (Function Type Expression)

- 함수 타입을 정의할 때 매개변수와 반환값의 타입을 명시적으로 지정하는 방법.
- 작성법 : `(매개변수: 타입) ⇒ 반환값 타입;`
- 재사용을 위해서 타입 별칭을 사용하는 것이 좋음

## 제네릭 타입(Generic Type)

- 제네릭 정의 : 타입을 미리 정해두지 않고 사용하는 시점에 지정할 수 있는 기능
- 여러 타입에 대해 동일한 구조나 동작을 보장할 수 있음
- 주로 배열, 객체 타입에 사용
- 작성법 : `<T> : 타입 파라미터(T는 타입 변수, 임의의 타입 의미)`

```tsx
// number, string, boolean 모든 타입을 설정 및 호출 시 지정하고 싶을 때
type Box<T> = {
  value: T;
};

// 문자열 담는 박스
const box1: Box<string> = {
  value: 'Hello',
};

// 숫자 담는 박스
const box2: Box<number> = {
  value: 23,
};
```

### 제네릭 함수(Generic Function)

두 객체 타입의 데이터를 전달받아 병합해서 반환하는 함수

```tsx
function merge<T, U>(a: T, b: U): T & U {
  return { ...a, ...b };
}

const mergedObj = merge({ name: '최유저' }, { age: 32 });
console.log(mergedObj);
```

## 유틸리티 타입(Utility Types)

### **파셜 `Partial<T>` 타입**

- T 타입의 모든 프로퍼티를 **선택적** 프로퍼티로 바꿔주는 타입
- 정보를 **수정**하는 함수

### 리콰이어드 **`Required<T>` 타입**

- T 타입의 모든 프로퍼티를 **필수** 프로퍼티로 바꿔주는 타입
- 정보를 **조회**하는 함수

### 리드온리 **`Readonly<T>` 타입**

- T 타입의 모든 프로퍼티를 **읽기 전용**으로 바꿔주는 타입
- React의 `props`나 `state`의 **불변성(Immutability)**을 지킬 때 유용

```tsx
function displayStudentInfo(student: Readonly<Student>) {
  // student.name = "변경";
  console.log('학생 정보 출력');
}

displayStudentInfo({
  id: 3,
  name: 'Hong',
  age: 22,
});
```

### 픽 **`Pick<T, K>` 타입**

- T 타입에서 K 프로퍼티들만 추출해 새로운 타입을 만들어주는 타입
- 목록(데이터)을 전달받아 출력하는 함수

```tsx
function printAttendanceList(students: Pick<Student, 'id' | 'name'>[]) {
  console.log('학생 정보 출력 중..');
}

printAttendanceList([
  { id: 1, name: 'Teddy' },
  { id: 2, name: 'Andy' },
  { id: 3, name: 'Jay' },
]);
```

### \*\*\*\*

### 오밋 **`Omit<T, K>` 타입**

- T 타입에서 K 프로퍼티들 **제외한 나머지** 프로퍼티들로 새로운 타입을 만들어주는 타입
- 새로운 정보를 등록하는 함수

```tsx
  interface Person {
    name: string;
    age: number;
    email: string;
  }

	type OmittedPerson = Omit<Person, 'email'>;

  OmittedPerson ({
    name: string;
    age: number;
  });
```

### 레코드 **`Record<K, T>` 타입**

- K 프로퍼티들을 키로 가지고 T 타입의 값들을 가지는 객체 타입을 만들어주는 타입

```tsx
type Role = 'admin' | 'user' | 'guest';
type RolePermissions = Record<Role, string[]>;

RolePermissions은 {
  admin: string[];
  user: string[];
  guest: string[];
}
타입과 동일
```

### **`Exclude<T, U>`, `Extract<T, U>` 타입**

- **Exclude** : T 타입(유니언)에서 U 타입의 프로퍼티를 제외한 나머지 타입들로 새로운 타입을 만들어주는 타입
- **Extract** : T 타입(유니언)에서 U 타입의 프로퍼티와 중복된 프로퍼티들만 추출하여 새로운 타입을 만들어주는 타입

```tsx
// 사용자 역할
type UserRole = 'SuperAdmin' | 'Admin' | 'Editor' | 'Viewer' | 'Guest';

// 직원 역할 타입
type StaffRole = Exclude<UserRole, 'Viewer' | 'Guest'>;
// StaffRole == 'SuperAdmin' | 'Admin' | 'Editor'

// 관리자 역할 타입
type AdminRole = Extract<UserRole, 'SuperAdmin' | 'Admin'>;
// AdminRole == 'SuperAdmin' | 'Admin'
```
