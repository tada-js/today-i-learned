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

- State
  - State(상태)만으로 React가 거의 설명되는 중요한 개념이다.
  - 계속해서 변화하는 특정상태
  - 상태에 따라 각각 다른 동작을 함
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

- 메모
  - React에서는 어떤 컴포넌트가 가진 State가 바뀌면 그 컴포넌트가 리렌더 된다.
  - React는 여러 개의 State를 하나의 컴포넌트가 가져도 문제가 되지 않는다.
  - State는 매우 짧은 코드와 유연한 문법으로 화면에 나타나는 데이터를 쉽게 교체하고 업데이트 할 수 있도록 도와준다.
  - State를 잘 활용하면 이벤트 동작을 다루는데 도움이 된다.

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
  return <div style={{ margin: 20, padding: 20, border: "1px solid gray" }}>{children}</div>;
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
          <input ref={authorInput} name="author" value={state.author} onChange={handleChangeState} />
        </div>
        <div>
          <textarea ref={contentInput} name="content" value={state.content} onChange={handleChangeState} />
        </div>
        <div>
          <select name="emotion" value={state.emotion} onChange={handleChangeState}>
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

  ***

## 데이터 추가하기

![](https://velog.velcdn.com/images/nu11/post/790e5813-cc04-409b-99e7-1e850e0f3444/image.png)

- 메모
  - React는 단방향으로만 데이터가 흐른다.
