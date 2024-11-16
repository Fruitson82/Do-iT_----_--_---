## 02. 리액트 동작 원리

### JSX 구문

- JSX는 XML 구조에 중괄호 { }를 사용해서 자바스크립트 코드를 감싸는 형태의 문법을 제공. 또한, 중괄호 안의 자바스크립트 코드는 반드시 return 키워드 없이 값만을 반환해야 함. (타입스크립트에서는 표현식이라고 함)
  ```jsx
  <p>
  	{/* string must be wrapped by Text */}
  </p>

  // 예시
  const hello = 'Hello world!'
  <p>{hello}</p>
  ```
- 부모와 자식 구조. 부모에서 함수 컴포넌트를 호출 시 자식에서 참조할 수 있는 속성을 props라고 함. 일종의 parameter.
  - JSX로 선언되는 컴포넌트의 속성은 XML을 확장한 것이기 때문에 string 타입으로 전달되어야 하는데 number나 boolean은 문자열이 아니기 때문에 중괄호 { }로 감싸야 함.

### Event

- on이 붙은 이벤트 헨들러는 가장 마지막에 설정된 콜백 함수를 호출한다.(addEventListener는 설정된 대로 모두 호출)
- 이벤트 버블링: 이벤트가 발생할 경우 해당 이벤트는 DOM 트리의 하위 요소에서 상위요소로 전달
- 관련 메서드
  - `e.stopPropagation()` 이벤트가 자식 요소에서만 처리되도록 하고, 부모로 이벤트가 전달되지 않게 하고 싶을 때 사용.
  - `e.preventDefault()` 브라우저에서 발생하는 기본 동작을 막고 싶을 때 사용. 예. 링크 클릭 시 페이지 이동, 폼 제출 시 페이지 리로드, 키보드 입력 시 기본 입력 동작 등.

### 생각

- 내용
  - e.stopPropagation()은 왜 필요한 걸까? (이벤트 버블링 존재의 이유)
  - 보통 이벤트를 설정할 때 부모로 전파되지 않아야 하지 않을까? 그렇다면 e.stopPropagation()이 기본이 되어야하고, 이벤트버블링 옵션이 선택적이어야 하지 않을까?
- 관련 서칭

  - 이벤트 버블링은 DOM 이벤트 처리의 기본 매커니즘. 부모 요소가 자식 요소의 이벤트를 감지할 수 있게 해주는 유용한 기능.
  - 이벤트 버블링이 유용한 경우
    - 부모에서 이벤트를 관리: 이벤트 처리를 중앙화하고, 자식 요소마다 따로 이벤트를 설정하지 않아도 되도록 함.
      ```jsx
      function Parent() {
        const handleClick = (e) => {
          console.log(`Clicked: ${e.target.tagName}`);
        };

        return (
          <div onClick={handleClick}>
            <button>Button 1</button>
            <button>Button 2</button>
          </div>
        );
      }
      ```
    - e.stopPropagation() 이 필요한 상황
      - 부모와 자식의 이벤트 처리 방식이 충돌할 때
      - 다른 이벤트 핸들러에 영향을 주지 않도록 할 때
    - 항상 e.stopPropagataion()을 쓰지 않는 이유?
      - 이벤트 버블링은 DOM 이벤트 처리 효율성을 위해 의도적으로 설계된 매커니즘
      - 필요 없는 경우에도 `e.stopPropagation()`을 사용하면 디버깅이 어려워진다.
      - 과도한 `e.stopPropagation()` 사용은 이벤트 처리를 복잡하게 만들고, 유지보수를 어렵게 하는 등 퍼포먼스에 영향을 줄 수 있다.

- 출처: https://chatgpt.com/share/67387a39-561c-800b-807a-27cdd98ceef7
