## **CSS (Cascading Style Sheet)**

- 콘텐츠(html)과 디자인(css)을 분리 관리
- 여러 페이지에서 사용 가능(테마, 템플릿 가능)
- 적용방법의 다양성
  - 내부 스타일(internal) : `<head>` 태그 하위 `<style>` 태그로 작성
  - 외부 스타일(external) : css 파일로 link 태그 작성
  - 인라인 스타일(inline) : html 태그 속성으로 `style=""` 직접 작성
- CSS 규칙
  - 선택자(selector)와 선언부(declaration)으로 구성
- 우선 순위
  - `!important` > inline > internal, external

### 외부 스타일 적용

- `<head>` 태그 내 link 태그 삽입
  - `<link rel="stylesheet" href="스타일시트 경로">`

### CSS 선택자

- 자손 선택자 표시 `>`
  `.class-name > h3`
- 인접 표시 `+`

  에밋(Emmet)문법 : `div>h3+ul>li*3`

- 속성 선택자

  특정 요소 내의 속성들을 가지고 선택하는 방법 `[ ]`

  - 표기 방식 예) `#id-name p[value] {}`
    - 속성 값에 `.` 또는 `공백`이 포함된 경우 `"."` 로 감싸줘야 함.
    - `~=` 또는 `*=` 의 경우 속성값을 `" "` 를 사용해야 함.
  - [속성=속성값] : 속성값과 **일치**하는 요소
  - [속성*=속성값] : 속성값이 **포함**되어 있는 요소
  - [속성^=속성값] : 속성값으로 **시작**되는 요소
  - [속성$=속성값] : 속성값으로 **끝**나는 요소
  - [속성~=속성값] : 속성값이 포함되어 있는 요소 (보류, 공백기준)
  - [속성|=속성값] : 속성값으로 시작되는 요소 (보류, 하이픈(-)기준)

- 가상 요소 (Pseudo-element)
  - `::before` 선택 요소 **맨 앞**에 가상 요소 생성
  - `::after` 선택 요소 **맨 뒤**에 가상 요소 생성
  - `::first-letter` 요소 **첫 글자** 스타일 적용
  - `::first-line` 요소 **첫 줄** 스타일 적용
  - `::selection` 텍스트 **드래그**로 선택 시 스타일
- 우선 순위
  1. `!important`
  2. 인라인 스타일 `<div style="color: #ccc;">`
  3. ID 선택자 `#id-name`
  4. 클래스, 속성, 가상 클래스 선택자
     1. `.class-name` `[type="text"]` `:hover`
  5. 태그(요소), 가상 요소 선택자 `<div>` `<p>` `::before`
  6. 전체 선택자 `*`

### ✔️ CDN 적용

- CDN (Contents Delivery Network)
  인터넷 url을 이용하여 외부 자원 사용하는 방식
- fontawesome 무료 아이콘
  ```bash
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/7.0.1/css/all.min.css" integrity="sha512-2SwdPD6INVrV/lHTZbO2nodKhrnDdJK9/kg2XD1r9uGqPo1cUbujc+IYdlYdEErWNu69gVcYgdxlmVmzTWnetw==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  ```
  ```bash
  @import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/7.0.1/css/all.min.css');
  ```

✅ vsCode html 태그 생성 팁

💡 **Tip**

1. p#$\*3 +엔터 : p 태그 id 입력 3개 생성
2. div.$\*5 + 엔터 : div 태그 클래스명 5개 생성
3. ul>li\*3 +엔터 : 목록 태그 3개 생성

### font 스타일

- `word-spacing` 단어 간격
- `text-decoration`
  - none : 없음
  - line-through : 취소선
  - underline : 밑줄
  - overline : 윗줄
- `text-indent` 들여쓰기
- `text-transform`
  - lowercase : 소문자
  - uppercase : 대문자
  - capitalize : 첫 글자만 대문자
- `text-shadow`
  - offset-x : 수평거리
  - offset-y : 수직거리
  - blur-radius : 그림자 번짐
  - color : 그림자 색상
  ```css
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
  ```

### **box 스타일**

- width / height
  - 너비와 높이 설정
  - px, vh, vw, % 등의 단위 사용 (%는 부모 요소 크기 기준)
  - 인라인 요소는 크기 지정 안됨.
- border
  - 테두리 종류, 두께, 색상 설정
  - `solid` `dotted` `dashed` `double` `groove` `ridge` `inset` `outset`
- box-sizing
  - 요소의 크기 계산 방식 설정
  - `content-box` 너비와 높이가 콘텐츠까지 범위 (기본값)
  - `padding-box` 너비와 높이가 내부 여백까지 범위
  - `border-box` 너비와 높이가 테두리까지의 범위
  - `* {box-sizing: border-box;}`

### display 스타일

- dispaly
  - `block` 블록 요소 - 한 줄로 배치 및 전체 너비 차지
  - `inline` 인라인 요소 설정 - 줄 바꿈 없이 너비와 높이 설정 불가
  - `inline-block` 인라인 요소지만 블록 요소로 취급 - 한 줄로 배치되지만 너비와 높이 설정 가능
  - `none` 숨기기

### float 스타일

- HTML 요소 배치
  - 블럭 : 기본적으로 위에서 아래로 배치
  - 인라인 : 기본적으로 왼쪽에서 오른쪽으로 배치

### flex 속성

- flex box 레이아웃
  - `flex-direction` item의 배치 방향

    - `row;` 수평 (기본값), Y축 기준
    - `row-reverse;` 수평 역방향
    - `column;` 수직, X축 기준
    - `column-reverse;` 수직 역방향

  - `flex-wrap` 컨테이너 범위 벗어날 경우
    - `nowrap;` 기본값
      - single-line 배치
      - 컨테이너 밖으로 나갈 수 있음
      - item 크기 변경 가능
    - `wrap;`
      - multi-line 배치
      - 컨테이너 밖으로 나가지 않음
      - item 크기 유지
  - `flex-flow`

    - flex-direction 과 flex-wrap 속성을 함께 표기 가능한 단축 속성
    - `flex-flow : row nowrap;`

  - `justify-content`정렬
    - `flex-start;`
    - `flex-end;`
    - `center;`
    - `space-between;` item 간의 공백 추가
    - `space-around;` item 마다 앞뒤에 공백 추가
    - `space-evenly;` item들 사이에 균일한 공백 추가
  - `flex-basis` 아이템 요소의 기본길이 설정
    - `flex-grow` 컨테이너 사이즈 커질수록 아이템 사이즈도 커질지 설정
      - `flex-grow:0;` 기본값, 커지지 않음
      - `flex-grow:1;` 아이템 사이즈도 같이 커짐, **1 이 활성화**
    - `flex-shrik` 컨테이너 사이즈가 작아질수록 아이템 사이즈도 작아질지 설정
      - `flex-shrik:0;` 활성화
      - `flex-shrik:1;` 기본값, 컨테이너 사이즈에 따라 아이템 사이즈도 작아짐, **1이 비활성화**
    - `flex: grow shrink basis;`
      - flex-grow, flex-shrink, flex-basis 를 한 번에 지정하는 단축 속
      - 예) `flex: 1 0 250px;`

💡 vsCode **Tip**

`.wrap7>.box{box$}*3` 클래스명 wrap7 하위 클래스명 box의 텍스트 boxn번 3개 생성

## Semantic 태그

의미를 명확하게 전달하는 HTML 태그 종류

- `header` `nav` `main` `aside` `footer` `section` `article` 등
- 웹 접근성(스크린리더 등 보조기기에서 구조 이해) 향상 목적, 코드 가독성 및 유지보수 향상, 검색 엔지 최적화(SEO) 도움

### Grid 스타일

- `display:gird;`
  - `grid-template-columns` 열
  - `grid-template-rows` 행
  - `fr` 비율, `repeat(반복횟수, 크기);` 함수로 설정, `minmax(최소,최대 크기);` 최소/최대 범위로 설정

### Flex 또는 Grid 의 정렬

- `justify-content` / `align-content` 컨테이너 기준

  ```css
  /* 아이템 가로 정렬*/
  justify-content: start;
  justify-content: end;
  justify-content: center;
  justify-content: space-between; /* 아이템 사이 공백 */
  justify-content: space-around; /* 아이템 앞뒤로 공백 */
  justify-content: space-evenly; /* 아이템 균등하게 공백 */
  ```

  ```css
  /* 아이템 세로 정렬 */
  align-content: start;
  align-content: end;
  align-content: center;
  align-content: space-between;
  align-content: space-around;
  align-content: space-evenly;
  ```

- `justify-items` / `align-items` 아이템 기준
  셀 내부에서 아이템 정렬 설정

### Transform 스타일

회전, 크기 조정, 기울임 등 변환 적용

- 변환 함수
  - translateX(x), translateY(y),translate(x, y)
    - x축, y축 이동
  - rotateX(angle), rotateY(angle), rotate(angle)
    - 회전 (90deg)
  - scaleX(x), scaleY(y), scale(x, y)
    - x축, y축 확대 또는 축소
  - skewX(x-angle), skewY(y-angle), skew(x-angle, y-angle)
    - x축, y축 방향 뒤틀기
- `transition` 움직임의 과정
  - **transition-property** 움직일 CSS 속성 이름
    - `transition-property: background-color,transform;`
  - **transition-duration** 움직이는 시간 설정
    - `transition-duration: 3s;`
  - **transition-timing-function** 움직이는 과정이 동작하는 방식
    - `transition-timing-function: liner;`

### Animation 스타일

`@keyframes` 규칙으로 애니메이션 시작과 끝 설정

- `animation-name`
- `animation-duration` 지속 시간 설정 (s 또는 ms)
- `animation-timing-function`
  - `ease;` 기본 (천천히→빠름 →천천히→끝)
  - `linear;` 일정한 속도
  - `ease-in;` 천천히→빠름
  - `ease-out;` 빠름→천천히→끝
  - `ease-in-out;` ease와 동
- `animation-delay` 시작 지연 시간
- `animation-iteratin-count` 반복횟수(숫자), infinite(반복)
- `animation-direction`
  - normal 기본
  - reverse 역방향
  - alternate 정방향 후 역방향 반복
  - alternate-reverse 역방향 후 정방향 반
- 단축 속성
  - `animation: animation-name(필수) duration(필수) [timing-function] [delay] [iteration-count] [direction]`
  - [ ] 생략 가능
  - 예 : `style="animation: pulse 1.5s 2s infinite;"`
