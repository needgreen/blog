# 스코프(Scope)가 뭘까?

- 스코프(Scope)는 변수가 어디까지 사용 가능한지를 정하는 범위다. 쉽게 말해 "이 변수를 여기서 쓸 수 있나?"를 결정하는 규칙이다.
  ```jsx
  function add(x, y) {
    console.log(x, y); // 2, 5 출력됨
    return x + y;
  }

  add(2, 5);
  console.log(x, y); // 에러 발생! x와 y는 함수 밖에서 못 씀
  ```
  위 예제에서 `x`와 `y`는 `add` 함수 안에서만 사용 가능하다. 함수 밖에서 사용하려고 하면 오류가 난다.

## 스코프의 두 가지 종류

### 1. 전역 스코프 - 어디서든 쓸 수 있는 변수

코드의 제일 바깥쪽에서 만든 변수는 **전역 변수**라고 부른다. 전역 변수는 코드 어디서든 사용할 수 있다.

```jsx
var myName = '김코드'; // 전역 변수

function sayHello() {
  console.log(myName); // "김코드" - 함수 안에서도 사용 가능
}

sayHello();
console.log(myName); // "김코드" - 바깥에서도 사용 가능
```

### 2. 지역 스코프 - 특정 영역에서만 쓸 수 있는 변수

함수 안에서 만든 변수는 **지역 변수**라고 부른다. 지역 변수는 그 함수 안에서만 사용할 수 있다.

```jsx
function greet() {
  var message = '안녕하세요'; // 지역 변수
  console.log(message); // "안녕하세요" - 함수 안에서는 OK
}

greet();
console.log(message); // 에러! 함수 밖에서는 사용 불가
```

## 스코프 체인 - 변수 찾기 규칙

JavaScript는 변수를 찾을 때 특별한 순서를 따른다. 이것을 **스코프 체인**이라고 부른다.

**규칙 : 안쪽 → 바깥쪽 순서로 찾는다**

```jsx
var color = '파란색'; // 전역 변수

function outer() {
  var color = '빨간색'; // outer 함수의 지역 변수

  function inner() {
    var color = '노란색'; // inner 함수의 지역 변수
    console.log(color); // 뭐가 나올까?
  }

  inner();
}

outer(); // "노란색" 출력
```

**찾는 순서 :**

1. inner 함수 안을 먼저 찾는다. → "노란색" 발견! → 여기서 끝.
2. (만약 없었다면) outer 함수 안을 찾는다.
3. (그래도 없다면) 전역 영역을 찾는다.

```jsx
var name = '전역';

function outer() {
  var age = 20;

  function inner() {
    console.log(name); // "전역" - 여기 없으니 위로 올라가서 찾음
    console.log(age); // 20 - 바로 위 outer에서 찾음
  }

  inner();
}

outer();
```

> 중요: 안쪽에서 바깥쪽 변수는 쓸 수 있지만, 바깥쪽에서 안쪽 변수는 못 쓴다!

## var, let, const의 차이

### var의 문제점 - 함수 밖 { } 를 무시한다.

`var`로 만든 변수는 함수 안에서만 지역 변수가 된다. if문이나 for문의 { } 안에서 만들어도 전역 변수가 되어버린다.

```jsx
var count = 1;

if (true) {
  var count = 10; // 새 변수가 아니라 위의 count를 바꿔버림!
}

console.log(count); // 10 - 의도하지 않게 바뀜
```

```jsx
for (var i = 0; i < 3; i++) {
  console.log(i); // 0, 1, 2
}

console.log(i); // 3 - for문 끝났는데도 i가 살아있음!
```

⚠️  **왜 이게 문제일까?**

보통 for문의 `i`는 반복문 안에서만 쓰려고 만든 변수다. 반복이 끝나면 더 이상 필요 없다.

하지만 `var`로 선언하면 for문이 끝나도 `i`가 살아있어서 다음과 같은 문제가 생긴다.

```jsx
// 첫 번째 for문
for (var i = 0; i < 3; i++) {
  console.log('첫 번째:', i); // 0, 1, 2
}

console.log(i); // 3

// 두 번째 for문 - 같은 i를 재사용!
for (var i = 0; i < 5; i++) {
  console.log('두 번째:', i); // 0, 1, 2, 3, 4
}

console.log(i); // 5 - 또 바뀜!

// **실수로 같은 변수명 i를 다시 사용하면 예상치 못한 버그가 발생할 수 있다.**
```

### let과 const - { } 를 제대로 인식한다

`let`과 `const`는 { } 블록을 제대로 인식한다. 블록 안에서 만든 변수는 블록 안에서만 쓸 수 있다.

```jsx
let count = 1;

if (true) {
  let count = 10; // 완전히 새로운 변수
  console.log(count); // 10
}

console.log(count); // 1 - 원래 값 그대로
```

```jsx
// let 사용 - 올바른 결과
for (let i = 0; i < 3; i++) {
  console.log(i); // 0, 1, 2
}

console.log(i); // 에러! for문 끝나면 i는 사라짐

**// let을 쓰면 i가 for문 안에서만 존재하고, for문이 끝나면 자동으로 사라진다.**
```

### 📌 **결론 : var는 쓰지 말고, let이나 const를 쓰자!**

## 렉시컬 스코프 - 어디서 정의했는지가 중요하다.

JavaScript는 함수가 **어디서 정의되었는지**를 본다. 어디서 호출되는지는 중요하지 않다.

```jsx
var fruit = '사과';

function outer() {
  var fruit = '바나나';
  inner(); // inner를 여기서 호출했지만...
}

function inner() {
  console.log(fruit); // 뭐가 나올까?
}

outer(); // "사과" 출력
inner(); // "사과" 출력
```

**이유 :** `inner` 함수는 전역에서 정의되었다. 그래서 `inner` 함수 안에서 변수를 찾을 때는 항상 전역을 본다. 어디서 호출하든 상관없다.

---

## 용어 정리

1. **스코프**: 변수를 쓸 수 있는 범위
2. **전역 변수**: 어디서든 사용 가능 (하지만 남용하면 안 좋음)
3. **지역 변수**: 특정 함수나 블록 안에서만 사용 가능
4. **스코프 체인**: 안쪽에서 바깥쪽으로 변수를 찾아간다.
5. **var**: 함수만 인식 (사용 비추천)
6. **let/const**: 블록({ })도 제대로 인식 (사용 권장)
7. **렉시컬 스코프**: 함수가 정의된 위치가 스코프를 결정한다.

## 💡 팁

**1. var 대신 let과 const를 쓰자.**

- 값이 바뀌는 변수: `let`
- 값이 바뀌지 않는 변수: `const`

**2. 전역 변수는 최소화하자.**

- 전역 변수가 많으면 실수로 값이 바뀔 수 있다.
- 함수 안에 변수를 만드는 습관을 들이자.

**3. 변수는 사용하기 직전에 선언하자.**

```jsx
// 좋은 예
function count() {
  let x = 10;
  let y = 20;
  let result = x + y;
  return result;
}
```

스코프는 JavaScript의 **가장 기초적이면서도 중요한 개념**이다.

처음에는 어렵게 느껴질 수 있지만, 예제 코드를 직접 실행해보면서 연습하고 익숙해지자!

---

# 전역 변수의 문제점

### 변수의 생명 주기란?

변수도 생물처럼 태어나고 죽는다. 이것을 **생명 주기**라고 부른다.

**지역 변수의 생명 주기**

- 함수가 실행되면 태어난다.
- 함수가 끝나면 죽는다.

```jsx
function sayHello() {
  var greeting = '안녕하세요'; // 함수 시작 시 태어남
  console.log(greeting);
} // 함수 끝날 때 죽음

sayHello();
console.log(greeting); // 에러! 이미 죽었음
```

**전역 변수의 생명 주기**

- 프로그램이 시작되면 태어난다.
- 웹페이지를 닫을 때까지 살아있다.

```jsx
var userName = '김코드'; // 웹페이지 열릴 때 태어남

function showName() {
  console.log(userName); // 여전히 살아있음
}

// 웹페이지를 닫기 전까지 계속 살아있음
```

### 전역 변수는 왜 문제일까?

전역 변수를 많이 쓰면 다음과 같은 문제가 생긴다.

**1. 모든 코드에서 값을 바꿀 수 있다.**

```jsx
var score = 100;

function gameA() {
  score = score - 10; // gameA에서 점수 감소
}

function gameB() {
  score = 0; // gameB에서 점수 초기화
}

gameA();
console.log(score); // 90
gameB();
console.log(score); // 0 - 의도치 않게 0이 됨!
```

전역 변수는 모든 함수에서 접근할 수 있어서, 어디서 값이 바뀌었는지 찾기 어렵다.

**2. 너무 오래 살아있다.**

```jsx
var tempData = '임시 데이터';
// ... 수많은 코드들 ...
// 1000줄 뒤에서도 여전히 메모리를 차지하고 있음
```

지역 변수는 함수가 끝나면 바로 사라져서 메모리를 절약하지만, 전역 변수는 프로그램이 끝날 때까지 메모리를 차지한다.

**3. 이름이 겹칠 수 있다.**

```jsx
var count = 1;

// ... 많은 코드 ...

// 다른 사람이 작성한 코드
var count = 100; // 실수로 같은 이름 사용!

console.log(count); // 100 - 원래 값이 사라짐
```

**4. 검색 속도가 느리다.**

JavaScript는 변수를 찾을 때 가까운 곳부터 찾는다. 전역 변수는 제일 마지막에 찾아서 조금 느리다.

### 전역 변수 사용을 줄이는 방법

전역 변수를 꼭 써야 할 이유가 없다면 지역 변수를 쓰는 것이 좋다. **전역 변수를 줄이는 방법**들을 알아보자.

**방법 1. 즉시 실행 함수 사용하기**

함수를 만들자마자 바로 실행하고 없애는 방법이다.

```jsx
(function () {
  var secret = '비밀 데이터'; // 이 안에서만 사용 가능
  console.log(secret);
})(); // 함수를 만들자마자 바로 실행

console.log(secret); // 에러! 외부에서 접근 불가
```

이렇게 하면 전역 변수를 만들지 않고도 코드를 실행할 수 있다.

**방법 2. 객체로 묶어서 관리하기**

여러 변수를 하나의 객체 안에 넣어서 관리하는 방법이다.

```jsx
// 나쁜 예 - 전역 변수 여러 개
var userName = '김코드';
var userAge = 20;
var userCity = '서울';

// 좋은 예 - 객체 하나로 관리
var user = {
  name: '김코드',
  age: 20,
  city: '서울',
};

console.log(user.name); // "김코드"
```

전역에 만드는 것은 `user` 하나뿐이라서 이름이 겹칠 위험이 줄어든다.

**방법 3. 함수로 감싸서 숨기기 (모듈 패턴)**

외부에 공개할 것만 공개하고, 나머지는 숨기는 방법이다.

```jsx
var password = (function () {
  var secret = '1234'; // 비밀번호는 숨김

  return {
    check: function (input) {
      if (input === secret) {
        return '비밀번호 맞음!';
      } else {
        return '비밀번호 틀림!';
      }
    },
    change: function (oldPw, newPw) {
      if (oldPw === secret) {
        secret = newPw;
        return '비밀번호 변경 완료!';
      } else {
        return '기존 비밀번호가 틀렸습니다!';
      }
    },
  };
})();

// 사용하기
console.log(password.secret); // undefined - 직접 볼 수 없음
console.log(password.check('1234')); // "비밀번호 맞음!"
console.log(password.check('5678')); // "비밀번호 틀림!"
console.log(password.change('1234', '9999')); // "비밀번호 변경 완료!"
console.log(password.check('9999')); // "비밀번호 맞음!"
```

- `secret` 변수는 함수 안에 숨겨져 있어서 외부에서 절대 못 봄 (즉시 실행 함수 사용)
- `check`와 `change` 함수로만 비밀번호 확인/변경 가능
- 이 방법을 쓰면 `password` 변수를 마음대로 바꿀 수 없고, 정해진 함수로만 조작할 수 있다.

**방법 4: ES6 모듈 사용하기 (최신 방법)**

파일마다 독립적인 공간을 만들어주는 방법이다.

```jsx
// num.js 파일
export var pi = 234;
export function add(a, b) {
  return a + b;
}

// num.js 파일
import { pi, add } from './num.js';
console.log(pi); // 234
console.log(add(2, 3)); // 5
```

- HTML에서 사용 방식

```html
<script type="module" src="main.js"></script>
```

각 파일이 독립적인 공간을 가지기 때문에 전역 변수 걱정 없이 코드를 작성할 수 있다.

## 마무리

- **변수의 생명 주기**
  - 지역 변수: 함수가 실행될 때 태어나고, 함수가 끝나면 죽는다.
  - 전역 변수: 프로그램이 시작될 때 태어나고, 웹페이지를 닫을 때 죽는다.
- **전역 변수의 문제점**
  1. 모든 코드에서 값을 바꿀 수 있어서 위험하다.
  2. 너무 오래 살아있어서 메모리를 낭비한다.
  3. 이름이 겹칠 위험이 크다.
  4. 검색 속도가 조금 느리다.
- **전역 변수를 줄이는 방법**
  1. 즉시 실행 함수로 감싸기
  2. 객체로 묶어서 관리하기
  3. 모듈 패턴 사용하기 (함수로 숨기기)
  4. ES6 모듈 사용하기 (가장 권장)
- **기억할 것**
  - 전역 변수는 꼭 필요할 때만 쓴다.
  - 가능하면 let, const로 지역 변수를 만든다.
  - 변수의 범위는 좁을수록 좋다.

### 📌 결론 : 전역 변수를 적게 쓰는 습관을 들이면 버그도 줄어들고 코드도 안전해진다!
