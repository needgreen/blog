## JavaScript Web

### DOM(Document Object Model)

문서 객체 모델

브라우저 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료 구조인 DOM을 생성한다

![DOM 생성 구조](.\images\dom.png)

DOM 생성 구조

### 노드(Node)

트리 하나하나를 구성하는 각각의 요소

- Document 노드 : 전체 문서를 나타내는 최상위 노드
- Element 노드 : 실제 HTML 태그
- Text 노드 : 태그 내부의 텍스트 내용
- Comment 노드 : HTML 주석
- 인스턴스 속성(property)
  - Element
    - className : class 속성(attribute)
    - id : id 속성(attribute)
    - innerHTML : 요소(element) 내에 포함된 HTML 또는 XML 마크업을 가져오거나 설정함
    - scrollHeight : 요소(element) 콘텐츠의 총 높이
    - scrollTop : 수직 스크롤 바의 위치
  - Node
    - firstChild : 현재 Node의 첫 번째 자식 Node 반환, 없으면 null 반환
    - lastChild : 현재 Node의 마지막 자식 Node 반환, 없으면 null 반환
    - previousSibling : 현재 Node의 이전 형제 Node 반환, 없으면 null 반환
    - nextSibling : 현재 Node의 다음 형제 Node 반환, 없으면 null 반환
    - parentElement : 현재 Node의 부모 Element 반환, 없으면 null 반환
    - parentNode : 현재 Node의 부모 Node 반환, 없으면 null 반환
    - textContent : 현재 Node와 그 자손의 텍스트 콘텐츠를 가져오거나 설정함

### DOM 객체를 취득하는 주요 메소드

- `document.getElementById(아이디);`
- `document.getElementsByClassName(클래스명);`
- `document.getElementsByTagName(tag명);`
- `document.querySelector(selectors)`
- `document.querySelectorAll(selectors)`

### 노드 탐색

- **자식 노드 탐색**

  1. `Node.prototype.childNodes` : 자식 노드(요소 노드, 텍스트 노드)를 탐색하여 NodeList에 담아 반환

  2. `Node.prototype.firstChild` : 첫번째 자식 노드(요소 노드, 텍스트 노드) 반환

  3. `Node.prototype.lastChild` : 마지막 자식 노드(요소 노드, 텍스트 노드) 반환

  4. `Element.prototype.children` : 자식 노드 중 요소 노드만 탐색하여 HTMLCollection에 담아 반환

  5. `Element.prototype.firstElementChild` : 첫번째 자식 요소 노드 반환

  6. `Element.prototype.lastElementChild` : 마지막 자식 요소 노드 반환

- **부모 노드 탐색**

  1. `Node.prototype.parentNode` : 부모 노드(요소 노드, 텍스트 노드)를 탐색하여 반환

  2. `Node.prototype.parentElement` : 부모 노드(요소 노드)를 탐색하여 반환

- **형제 노드 탐색**

  1. `Node.prototype.previousSibling` : 형제 노드 중 자신의 이전 형제 노드(요소노드, 텍스트노드)를 탐색하여 반환

  2. `Node.prototype.nextSibling` : 형제 노드 중 자신의 다음 형제 노드(요소노드, 텍스트노드)를 탐색하여 반환

  3. `Element.prototype.previousElementSibling` : 형제 요소 노드 중 자신의 이전 형제 요소 노드를 탐색하여 반환

  4. `Element.prototype.nextElementSibling` : 형제 요소 노드 중 자신의 다음 형제 요소 노드를 탐색하여 반환

### 요소가 가지는 속성 확인 제어

- HTML Attribute : HTML 태그에 명시되어 있는 속성을 의미
  - 예) type, name, id, class, src 등
- 속성 제어하기
  - 속성값 가져오기 `Element.prototype.getAttribute(attribute)`
    ```jsx
    console.log($image.getAttribute('src'));
    ```
  - 속성값 수정하기 `Element.prototype.setAttribute(attribute, value)`
    ```jsx
    document.querySelector('#cook').setAttribute('checked', 'checked');
    ```
  - 속성 삭제하기 `Element.prototype.removeAttribute(attribute)`
    ```jsx
    document.querySelector('#cook').removeAttribute('checked');
    ```
  - 속성 존재 여부 확인 `Element.prototype.hasAttribute(attribute)`

### 노드 생성

- `document.createElement(tagName)` → 요소 노드 생성
- `document.createTextNode(text)` → 텍스트 노드 생성
  ```jsx
  const div = document.createElement('div');
  const text = document.createTextNode('Hello World');
  ```

### 노드 복사 / 삭제 / 변경

**1. cloneNode() - 노드 복사**

`Node.prototype.cloneNode([deep: true|false])`

- **기능**: 노드의 사본을 생성하여 반환
- **매개변수 deep** (선택):
  - `true`: 깊은 복사 - 모든 자손 노드 포함
  - `false` (기본값): 얕은 복사 - 노드 자신만 복사

**2. replaceChild() - 자식 노드 교체**

`Node.prototype.replaceChild(newChild, oldChild)`

- **기능**: 자식 노드를 새 노드로 교체
- **결과**: oldChild는 DOM에서 제거됨

**3. removeChild() - 자식 노드 삭제**

`Node.prototype.removeChild(child)`

- **기능**: 특정 자식 노드를 DOM에서 제거

**4. remove() - 자기 자신 삭제**

`Element.prototype.remove()`

- **기능**: 호출한 노드를 DOM에서 직접 제거
- **특징**: 부모 노드를 거치지 않고 바로 제거 가능

**핵심 차이점**: `removeChild()`는 부모에서 자식을 제거, `remove()`는 자기 자신을 제거

## 이벤트(event)

사용자의 액션(클릭, 키보드 입력, 마우스 움직임 등)에 의해 발생

- 이벤트 핸들러 : 이벤트가 발생하면 실행되는 코드
- 이벤트 리스너 : 특정 이벤트가 발생했는지 감지하는 역할

1. **고전 이벤트 모델**

   1. 익명함수 `요소.onclick = function(){ }`
   2. 화살표함수 `요소.onclick = () => { }`
   3. 기명함수 `요소.onclick = eventHandler;`

1. **인라인 이벤트 모델**

   1. 이벤트 할당 방식 예시

   ```
   <button type="button" onclick="inline()">
          function inline(){

          }
   ```

1. **표준 이벤트 모델**

   EventTarget 인터페이스의 addEventListener() 메소드를 호출하는 방식

   1. 익명 함수
      `요소.addEventListener('click', function(){ })`
   2. 화살표 함수
      `요소.addEventListener('click', () => { })`
   3. 기명 함수

      ```jsx
      function eventHandler() {}
      요소.addEventListener('click', eventHandler);
      ```

   4. 예시

      ```jsx
      <h2>표준 이벤트 모델 방식</h2>

          <button id="btn4">표준이벤트모델1</button>
          <button id="btn5">표준이벤트모델2</button>
          <button id="btn6">표준이벤트모델3</button>

          <script>
            // 익명 함수
            $("#btn4").addEventListener("click", function () {
              // (감지할 이벤트 종류(이벤트명은 정해져 있음), 이벤트 핸들러(함수))
              console.log("btn4 클릭!!");
            });
            $("#btn4").addEventListener("click", function () {
              alert("btn4 클릭!!");
            });
            // 화살표 함수
            $("#btn5").addEventListener("mouseenter", () => {
              console.log("btn5 마우스 들어감");
            });
            // 기명 함수
            $("#btn6").addEventListener("dblclick", action); //dbclick => 더블클릭 이벤트명
          </script>
      ```
