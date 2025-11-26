## HTML

HTML (Hyper Text Markup Language): 웹 페이지를 만들기 위한 언어

- **Hyper Text**: 문서들이 링크로 연결되어 비선형적으로 정보를 탐색할 수 있는 텍스트
- **Markup**: 문서의 구조와 표현을 위해 태그를 사용해 표시
- **Language**: 컴퓨터가 이해할 수 있는 규칙과 문법을 가진 언어

## HTML 구성 요소

- 태그(tag): HTML에서 <와 >로 묶인 명령어
- 속성(attribute): 요소의 시작 태그에 사용, 명령어를 구체화하는 역할
- 속성값(argument): 속성에 부여하는 값, 작은따옴표(' ') 또는 큰따옴표(" ") 안에 표기
- 요소(element): 시작 태그와 종료 태그를 포함한 전체 HTML 구조

## HTML 태그

### Text 태그

- heading 태그

1. 제목을 나타내는 태그
2. 블럭요소
3. <h1> ~ <h6>
4. 숫자가 작아질수록 글자크기+굵기가 커짐

- 텍스트 효과를 지정하는 태그

1. 인라인요소
2. 종류
   → 글자를 굵게 표현 : b, strong
   → 글자를 기울여 표현 : i, em(empasis)
   → 글자에 밑줄로 표현 : u, ins
   → 글자에 취소선 표현 : s, del
   → 글자에 첨자 표현 : sub, sup
   → 글자에 형광펜 표현 : mark
   → 글자를 작게 표현 : small (big은 html5아님)
   → 글자의 약어를 표현 : abbr

### Table 태그

- 태그 종류
  → <table></table> 표 생성
  → <tr></tr> 표 내의 행
  → <th></th> 표 내의 제목 셀 (글자 가운데 정렬, 굵게 표현)
  → <td></td> : 표 내의 일반 셀
  - 셀 병합하기
    1. 셀 태그(td, th) 내의 속성 병합
    2. 병합 속성
       → colspan : 좌우 셀을 병합
       → rowspan : 상하 셀을 병합
    3. 작성 예시
        <td colspan="2"></td> : 좌우 2개의 셀을 병합하여 표현
        <td rowspan="2"></td> : 상하 2개의 셀을 병합하여 표현

### 미디어 태그

- <audio></audio> 태그
  ```bash
  <audio [controls] [autoplay] [loop] [muted]>
          <source type="" src="">
          브라우저가 audio 태그를 지원하지 않을 경우 대체 텍스트 문구
  </audio>
  ```
  - 주요 속성
    → src : 오디오 파일 경로
    → autoplay : 자동 재생 여부  
     → loop : 반복 재생 여부
    → muted : 음소거 여부
    → controls : 오디오 컨트롤 패널(재생, 중지, 소리조절 등) 노출 여부
  - 오디오의 content-type (MIME type)
    → audio/wav
    → audio/mp3
    → audio/ogg
- <video></video>
  ```bash
  <video [controls] [autoplay] [loop] [muted] width="" height="" poster="">
          <source type="" src="">
      </video>
  ```
  - 주요 속성
    → src : 비디오 파일 경로
    → autoplay : 자동 재생 여부  
     → loop : 반복 재생 여부
    → muted : 음소거 여부
    → controls : 비디오 컨트롤 패널(재생, 중지, 소리조절 등) 노출 여부
    → width : 비디오 너비
    → height : 비디오 높이
    → poster : 썸네일 이미지 파일 경로
  - 비디오의 content-type (MIME type)
    → video/mp4
    → video/webm
    → video/ogg

### form 태그

`action="서버주소"` `method="전송방식"`

`<input>` 태그로 입력 박스 생성

- 그룹 태그 (테두리로 표시)
  `<fieldset></fieldset>`
- 그룹 제목 표시 태그
  `<legend></legend>`
- <datalist> 태그
  1. 콤보 상자 태그 (입력란 + 목록 상자)
  2. 사용자가 직접 입력도 할 수 있고 목록 중 선택도 가능하게 표현 가능
  3. 인라인 요소
  4. HTML5부터 사용 가능
  5. 형식

```bash
<input type="" list="datalist아이디">
      <datalist id="아이디">
        <option></option>
        <option></option>
        <option></option>
      </datalist>
```

### type

- 라디오 버튼

  - name 속성 값이 동일한 경우 한 개만 선택 가능 (그룹 내 1개 선택)
  - `input type="radio"`
  - `<lable>` 태그를 사용으로 포커싱
    - 속성 `for="속성이름(아이디와 동일)"`
  - `id="" value="" name=""` `checked`
    - `id` 와 `for` 는 서로 매칭
    - `value` DB or서버 저장
    - `name` 서버에서 그룹

- 체크박스

  - `input type="checkbox"`
  - `<lable>` 태그를 사용으로 포커싱
    - 속성 `for="속성이름(아이디와 동일)"`
  - `id="" value="" name=""`

- 그외
  - `type="color"`
  - `type="file"`
  - `type="hidden"` 숨긴 정보 설정 시 `value=""` 데이터 값 필수 입력
  - `type="button"` 의 텍스트 표시는 `value=""` 으로 표시할 수 있음
- 버튼
  `<button></button>` 태그 기본 type은 submit 이므로 type 입력 습관으로 할 것
