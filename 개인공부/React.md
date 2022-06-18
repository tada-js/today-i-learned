# JavaScript 학습 및 복습

## React.js

- 리액트가 필요한 이유
  - 첫 번째
    - 중복 코드 작성. 산탄총 수술(Shotgun Surgery)을 해결해준다.
    - 컴포넌트화 방식
    - React는 컴포넌트 기반의 UI 라이브러리
  - 두 번째
    - 명령형 프로그래밍 ⇒ 절차를 하나하나 다 나열 해야함
      - jQuery
    - 선언형 프로그래밍 ⇒ 그냥 목적을 바로 말함
      - React
  - Virtual DOM
- **리액트를 쓰는 이유**
  - Single Page Applicatiojn 만들때 쓴다.
    - 자바스크립트로도 가능하나 코드가 길고 복잡해진다.
  - React 쓰면 html 재사용 편리
  - 같은 문법으로 앱개발 가능
- **js 파일에 html 짜도 웹페이지가 구현되네?**
  - `App.js`에 있던 html들을 `index.html` 이라는 메인페이지로 처리해주기 때문
    아마 `index,.js` 가 처리해줌
- React App을 만드는 방법
  - React.js
    - Node 기반의 JavaScript UI 라이브러리
  - Webpack
    - 다수의 자바스크립트 파일을 하나의 파일로 합쳐주는 모듈 번들 라이브러리
  - Babel
    - JSX 등의 쉽고 직관적인 자바스크립트 문법을 사용할 수 있도록 해주는 라이브러리
      위의 환경세팅을 언제 다 하지? ⇒ Create React App(이미 세팅 완료된 패키지)
  - Boiler Plate
    - 이미 세팅 완료된 패키지
    - 마치 보일러를 찍어내듯, 서비스를 개발할 수 있는 빵 틀의 역할을 하는 패키지를 의미함
    - `npx create-react-app "프로젝트명"`
- 메모
  - React App은 Node.js 기반의 웹 서버 위에서 동작하고 있다.

## JSX

- **JSX 문법**

  - class 넣을 땐 `className`
  - 변수넣을 땐 `{중괄호}`
    - 변수에 있던걸 html에 꽂아넣는 작업 ⇒ 데이터바인딩 이라고 함
    - 실무에서 이런 식으로 작업함.
      ex) 서버에서 데이터 가져와서 `<html>` 사이에 넣어주세요~
  - style 넣을 땐 `style={ {스타일명: "값"} }`
    - css 파일 열기 귀찮을 때 쓰면 된다.
    - font-size처럼 속성명에 대쉬기호를 쓸 수 없다.
      camelCase 작성

- React.Fragment

  - React.Fragment를 통해 최상위 태그를 대체할 수 있다.
  - 리액트의 기능이기 때문에 `import` 해줘야 한다. ⇒ `import React from "react";`

    ```jsx
    import React from "react";
    import "./App.css";

    import MyHeader from "./MyHeader";

    function App() {
      return (
        <React.Fragment>
          <MyHeader />
          <header className="App-header">
            <h2>안녕 리액트</h2>
          </header>
        </React.Fragment>
      );
    }

    export default App;
    ```

- CSS

  - 인라인 스타일

    ```jsx
    import React from "react";
    // import "./App.css";

    import MyHeader from "./MyHeader";

    function App() {
      const style = {
        App: {
          backgroundColor: "black",
        },
        h2: {
          color: "white",
        },
        bold_text: {
          color: "green",
        },
      };

      return (
        <div style={style.App}>
          <MyHeader />
          <h2 style={style.h2}>안녕 리액트</h2>
          <b style={style.bold_text} id="bold_text">
            React.js
          </b>
        </div>
      );
    }

    export default App;
    ```

  - css 스타일

    ```jsx
    // App.js

    import React from "react";
    import "./App.css";

    import MyHeader from "./MyHeader";

    function App() {
      return (
        <div className="App">
          <MyHeader />
          <header className="App-header">
            <h2>안녕 리액트</h2>
            <b id="bold_text">React.js</b>
          </header>
        </div>
      );
    }

    export default App;
    ```

    ```css
    /* App.css */

    .App {
      background-color: black;
    }

    h2 {
      color: white;
    }

    #bold_text {
      color: green;
    }
    ```

- 조건부 렌더링

  - 삼항 연산자를 사용한 조건부 렌더링
    ```jsx
    {
      number;
    }
    는: {
      number % 2 === 0 ? "짝수" : "홀수";
    }
    ```

- 메모
  - 최상위 컴포넌트를 통상 `<App />` 으로 둔다.
  - 모든 태그는 닫힘 태그 사용
  - JSX에서는 `class` 라는 이름은 자바스크립트 예약어이기 때문에 사용할 수 없다.
    따라서 `className`을 사용한다.

## State(상태)

- **State**

  - State(상태)만으로 React가 거의 설명되는 중요한 개념이다.
  - 계속해서 변화하는 특정상태
  - 상태에 따라 각각 다른 동작을 함
  - 자료를 잠깐 저장할 때는 변수를 사용한다.
    `let post = "강남 우동 맛집";`
  - 리액트에서는 자료를 잠깐 저장하고 싶다? 그러면 State를 써도 된다.
  - 만드는 법

    1. `import { useState }`
    2. `useState(보관할 자료)`
    3. `let [작명, 작명]`
       ⇒ `let [a, b] = useState("강남 우동 맛집");`
       a에는 state에 보관했던 자료 나옴 ⇒ `<h4> {a} </h4>`
       b에는 state 변경 도와주는 함수

    - Destructuring 문법

      ```jsx
      let num = [1, 2];
      let [a, b] = [1, 2];
      // let a = num[0];
      // let b = num[1];

      // let [a, b] = useState("강남 우동 맛집");
      ```

  - 왜 State를 쓸까?
    - 일반 변수는 갑자기 변경되면 html에 자동으로 반영이 안된다.
    - state는 갑자기 변경되면 state를 쓰던 html은 **자동 재렌더링**이 된다.
    - 즉, 변동시 자동으로 html에 반영되게 만들고 싶으면 state 쓰자!
  - State 변경하는 방법
    - `state변경함수(새로운state)` ⇒ `onClick={() => {따봉변경(따봉 + 1);}}`
    - `따봉 = 따봉 +1` 처럼 등호로 변경 금지
  - State변경함수 특징
    - 기존state == 신규state의 경우 변경을 안함.
  - array/object 특징
    - array/object 담은 변수에는 화살표만 저장된다.
      - `let arr = [1,2,3]` 의 경우 `[1,2,3]` 이 어디에 있는지 알려주는 화살표만 변수에 저장된다.
      - `arr[0] = 100` 의 경우에는 array를 수정한 것이고 변수에 있던 화살표는 수정이 안된다.
        ```jsx
        // arr와 copy에는 같은 화살표를 가짐
        let arr = [1, 2, 3];
        let copy = arr;
        console.log(copy === arr); // true
        ```
        ```jsx
        // [...]을 통해 arr와 copy는 다른 화살표를 가짐
        let arr = [1, 2, 3];
        let copy = [...arr];
        console.log(copy === arr); // false
        ```
      - 즉, state가 array/object면 `[...]` 을 사용해서 독립적 카피본을 만들어 수정해야 한다.
      - `[...]` 의 점 3개는 뭐냐면 spread operator 문법이라고 괄호를 벗겨주는 연산자다. array나 object 자료형을 복사할 때 많이 사용한다.

- useState

  - State는 리액트의 기능이기 때문에 useState 메서드를 import 해줘야 된다.
    `import React, { userState } from "react"`
  - 예제1 (count 버튼)

    ```jsx
    import React, { useState } from "react";

    const Counter = () => {
      const [count, setCount] = useState(0);

      const onIncrease = () => {
        setCount(count + 1);
      };

      const onDecrease = () => {
        setCount(count - 1);
      };

      return (
        <div>
          <h2>{count}</h2>
          <button onClick={onIncrease}>+</button>
          <button onClick={onDecrease}>-</button>
        </div>
      );
    };

    export default Counter;
    ```

- 부모 → 자식 state 전송하는 방법

1. <자식컴포넌트 작명={state 이름}>
2. props 매개변수 등록 후 props.작명 사용
3. props 전송은 부모 → 자식만 가능

```jsx
function App() {
  return (
    {modal === true ? <Modal 글제목={글제목} /> : null};
  );
}

function Modal(props) {
  return (
    <div className="modal">
      <h4>{props.글제목}</h4>
      <p>날짜</p>
      <p>상세내용</p>
    </div>
  );
}
```

- `<input />`에 뭔가 입력시 코드 실행하고 싶으면?

- onChange / onInput

- 아래 코드에서 `<span>`을 눌러도 왜 모달창이 뜨는가?

```jsx
{
  글제목.map(function (a, i) {
    return (
      <div className="list">
        <h4
          onClick={() => {
            setModal(true);
            setTitle(i);
          }}
        >
          {글제목[i]}
          <span
            onClick={() => {
              let copy = [...따봉];
              copy[i] = copy[i] + 1;
              따봉변경(copy);
            }}
          >
            👍
          </span>
          {따봉}
        </h4>
        <p>2월 17일 발행</p>
      </div>
    );
  });
}
```

클릭이벤트는 상위 html로 퍼지는 이벤트 버블링 때문이다.
상위 html로 퍼지는이벤트 버블링을 막고 싶으면 `e.stopPropagation()` 을 추가하면 된다.

- 메모
  - React에서는 어떤 컴포넌트가 가진 State가 바뀌면 그 컴포넌트가 리렌더 된다.
  - React는 여러 개의 State를 하나의 컴포넌트가 가져도 문제가 되지 않는다.
  - State는 매우 짧은 코드와 유연한 문법으로 화면에 나타나는 데이터를 쉽게 교체하고 업데이트 할 수 있도록 도와준다.
  - State를 잘 활용하면 이벤트 동작을 다루는데 도움이 된다.

## 컴포넌트

- **컴포넌트 만드는 방법**

  1. 다른 함수 바깥에 function 만들고 네이밍은 PascalCase로 한다.
  2. return() 안에 html 담기
     return() 내부 최상위 루트에는 태그 하나만 사용(병렬 사용 못함)
  3. <함수명></함수명> 쓰기. <함수명 />도 가능.

     ```jsx
     // Modal 컴포넌트
     function Modal() {
       return (
         <div className="modal">
           <h4>제목</h4>
           <p>날짜</p>
           <p>상세내용</p>
         </div>
         // <div>이렇게 최상위 루트에 병렬 사용 못함</div>
       );
     }
     ```

  4. arrow function도 사용 가능하다.

     ```jsx
     function Modal() {
       return <div></div>;
     }

     const Modal = () => {
       return <div></div>;
     };
     ```

- **어떤걸 컴포넌트로 만들면 좋은가?**

  - 기준은 없다. 함수 문법과 마찬가지다.
    함수 문법을 언제 쓰는가? 긴 코드 축약, 코드 재사용, 복잡한 코드를 작은 기능으로 나눌 때 쓰는데 컴포넌트도 마찬가지다.
  - 사이트에 반복해서 출현하는 HTML 덩어리들 ⇒ 반복적인 HTML 축약할 때
  - 내용이 자주 변경될 것 같은 HTML 부분을 잘라서 컴포넌트화
  - 다른 페이지를 만들고 싶다면 그 페이지의 HTM 내용을 하나의 컴포넌트화
  - 큰 페이지들
  - 다른 팀원과 협업할 때 웹페이지를 컴포넌트 단위로 나눠서 작업하기도 함.

- **컴포넌트의 단점**
  - state 가져다쓸 때 문제가 생긴다.
    - A 함수에 있던 변수는 B 함수에서 쓸 수 없기 때문.
- **동적인 UI 만드는 step**

  1. html, css로 미리 디자인 완성

     ```jsx
     function Modal() {
       return (
         <div className="modal">
           <h4>제목</h4>
           <p>날짜</p>
           <p>상세내용</p>
         </div>
       );
     }
     ```

  2. UI의 현재 상태를 state로 저장
     1. `let [modal, setModal] = useState(true);`
        true, false, “열림”, “닫힘” 등..
        형식은 자유, 모달창 상태 표현만 가능하면 된다.
  3. state에 따라 UI가 어떻게 보일지 저장(조건문 등으로)

- map() 기능

  - array 자료 갯수만큼 함수안의 코드 실행해줌
  - 함수의 파라미터는 array안에 있던 자료임
  - return에 뭐 적으면 array에 담아준다.

## Props

- 컴포넌트에 데이터를 전달하는 방법
- 리액트의 컴포넌트는 본인이 관리하고 본인이 가진 state가 바뀔 때마다 리렌더가 되고,
  나에게 내려온 props가 바뀔 때마다 리렌더가 되고,
  둘 다 아니여도 내 부모가 리렌더가 되면 나도 리렌더가 된다.
- props 기능은 컴포넌트를 다른 컴포넌트의 props로 전달할 수 있다.

```jsx
// Counter.js

import React, { useState } from "react";
import OddEvenResult from "./OddEvenResult";

const Counter = ({ initialValue }) => {
  const [count, setCount] = useState(initialValue);

  const onIncrease = () => {
    setCount(count + 1);
  };

  const onDecrease = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
      <OddEvenResult count={count} />
    </div>
  );
};

Counter.defaultProps = {
  initialValue: 0,
};

export default Counter;
```

```jsx
// Container.js

const Container = ({ children }) => {
  return (
    <div style={{ margin: 20, padding: 20, border: "1px solid gray" }}>
      {children}
    </div>
  );
};

export default Container;
```

```jsx
// App.js

import React from "react";
import Container from "./Container";
import Counter from "./Counter";
// import "./App.css";

import MyHeader from "./MyHeader";

function App() {
  const number = 5;

  const counterProps = {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: 5,
  };
  return (
    <Container>
      <div>
        <MyHeader />
        <Counter {...counterProps} />
      </div>
    </Container>
  );
}

export default App;
```

## useRef

- HTML DOM 요소에 접근할 수 있는 React의 기능인 useRef
- 예제1

  ```jsx
  import { useRef, useState } from "react";

  const DiaryEditor = () => {
    const authorInput = useRef();
    const contentInput = useRef();
    const [state, setState] = useState({
      author: "",
      content: "",
      emotion: 1,
    });

    const handleChangeState = (e) => {
      setState({
        ...state,
        [e.target.name]: e.target.value,
      });
    };

    const handleSubmit = () => {
      if (state.author.length < 1) {
        authorInput.current.focus();
        return; // 더 이상 진행이 안되로록 리턴
      }
      if (state.content.length < 5) {
        contentInput.current.focus();
        return;
      }
      alert("저장 성공");
    };

    return (
      <div className="DiaryEditor">
        <h2>오늘의 일기</h2>
        <div>
          <input
            ref={authorInput}
            name="author"
            value={state.author}
            onChange={handleChangeState}
          />
        </div>
        <div>
          <textarea
            ref={contentInput}
            name="content"
            value={state.content}
            onChange={handleChangeState}
          />
        </div>
        <div>
          <select
            name="emotion"
            value={state.emotion}
            onChange={handleChangeState}
          >
            <option value={1}>1</option>
            <option value={2}>2</option>
            <option value={3}>3</option>
            <option value={4}>4</option>
            <option value={5}>5</option>
          </select>
        </div>
        <div>
          <button onClick={handleSubmit}>일기 저장하기</button>
        </div>
      </div>
    );
  };
  export default DiaryEditor;
  ```

![](https://velog.velcdn.com/images/nu11/post/8e0c20e9-0227-4050-bf2f-dafa8d72159a/image.png)

`content` 가 글자수가 5글자 이하일 때 `content` 영역이 `focus` 된다.
`alert` 으로 “5글자 이상 입력”으로 나타내는 건 UX경험으로 좋지 않기 때문에 이와 같은 방식을 사용하자.

## 리스트 렌더링(조회)

- React에서 리스트 사용하기
  - `Array.map((it)⇒<Component key={it.id}{…it}/>)`
  - React에서 배열은 게시글이나 리스트, 피드를 표시하는데 자주 사용된다.
- 메모

  - 렌더링 ⇒ 화면에 표시한다.
  - defaultProps ⇒ undifined으로 전달될 거 같은 props들을 기본값을 설정 해준다.

    ```jsx
    import "./App.css";
    import DiaryEditor from "./DiaryEditor";
    import DiaryList from "./DiaryList";

    const dummyList = [
      {
        id: 1,
        author: "사용자",
        content: "내용",
        emotion: 5,
        created_date: new Date().getTime(),
      },
      {
        id: 2,
        author: "사용자2",
        content: "내용2",
        emotion: 5,
        created_date: new Date().getTime(),
      },
      {
        id: 3,
        author: "사용자3",
        content: "내용3",
        emotion: 2,
        created_date: new Date().getTime(),
      },
    ];

    const App = () => {
      return (
        <div className="App">
          <DiaryEditor />
          <DiaryList diaryList={undefined} />
        </div>
      );
    };

    DiaryList.defaultProps = {
      diaryList: [],
    };
    export default App;
    ```

  - 현재 시간을 알아보기 쉽게 변환 ⇒ `new Date(*created_date*).toLocaleString()`
    - `toLocaleString()` 사용안함
      ![](https://velog.velcdn.com/images/nu11/post/850ef410-17da-4bb9-bf1d-1b8229e42346/image.png)
    - `toLocaleString()` 사용
      ![](https://velog.velcdn.com/images/nu11/post/840c7d3c-949e-44f7-9bd6-664899c1a1a8/image.png)

## 데이터 추가하기

![](https://velog.velcdn.com/images/nu11/post/790e5813-cc04-409b-99e7-1e850e0f3444/image.png)

- 메모
  - React는 단방향으로만 데이터가 흐른다.

## 메모

- node_modules 폴더
  - 라이브러리 코드 보관함
- public 폴더
  - static 파일 모아놓는 곳
- src 폴더
  - 코드 짜는 곳(소스코드 보관함)
- package.json
  - 프로젝트 정보
- `/** eslint-disable **/`
  - Lint 끄는 기능(Warning 메시지)
- array/object 다룰 때 원본은 보존하는게 좋음
