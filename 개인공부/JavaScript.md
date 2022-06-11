# JavaScript 학습 및 복습

---

## 실행 컨텍스트(execution context)

- Execution ⇒ 실행, Context ⇒ 문맥/맥락/환경
  - Context란 코드의 배경이 되는 조건/환경
- 동일한 조건을 지닐 수 있는 조건
  - 전역공간
  - 함수
  - ~~eval~~
  - module
- **즉, 실행 컨텍스트는 함수를 실행할 때 필요한 조건/환경정보를 담은 객체**
- 예제1

  ```jsx
  // 코드의 결과를 예측해 보자
  var a = 1;
  function outer() {
    console.log(a); // 1

    function inner() {
      console.log(a); // 2
      var a = 3;
    }

    inner();

    console.log(a); // 3
  }
  outer();
  console.log(a); // 4
  ```

![](https://velog.velcdn.com/images/nu11/post/750a7140-0fec-46d2-b53d-6a4a7c92dec6/image.png)

    1. 제일 먼저 전역 컨텍스트가 열림 ⇒ 전역 공간을 한 줄 한 줄 실행
    2. `outer()` 실행 함수를 만남
    3. `outer` 함수를 호출함에 따라서 `outer` 함수의 실행 컨텍스트가 열리면서 `outer` 함수의 내부에 대해서 한 줄 한 줄 실행하게 됨.
    4. 주석으로 표시된 첫 번째 `console.log` 가 출력.
    5. `inner` 함수 선언부는 넘어가고 `inner` 실행 함수를 만나 그제서야 `inner` 함수 실행 컨텍스트가 열림
    6. 두 번째 `console.log` 출력
    7. `inner` 함수 호출이 종료되고 세 번째 `console.log` 출력
    8. `outer` 함수 실행 컨텍스트가 종료되면 네 번째 `console.log` 출력

- 제일 먼저 들어왔던게 제일 마지막에 빠지고, 제일 마지막에 들어온 게 제일 먼저 빠지는 개념을 “스택”이라고 한다.
  (마지막으로 제일 처음에 생성되었던 전역 컨텍스트가 사라지는)

![](https://velog.velcdn.com/images/nu11/post/af6d5e5e-edc6-4d8c-9d5b-3d0e5c2f1eca/image.png)

    코드 실행에 관여하는 스택을 “콜스택” 이라고 한다.

- 콜스택이란 현재 어떤 함수가 동작중인지, 다음에 어떤 함수가 호출될 예정인지 등을 제어하는 자료구조다.
- 실행 컨텍스트에는 세 가지 환경 정보들이 담긴다.
  - VariableEnvironment
    - 현재 환경과 관련된 식별자 정보들이 담긴다.
    - 식별자 정보를 수집하는 용도로만 쓰인다.
  - LexicalEnvironment
    - 현재 환경과 관련된 식별자 정보들이 담긴다.
    - 각 식별자에 담긴 데이터를 추적하는 용도로 쓰인다.
  - ThisBinding
  - 컨텍스트 내부 코드들을 실행하는 동안에 변수의 값들이 변화가 생기면 그 값이 LexicalEnvironment에만 실시간으로 반영된다. 즉, VariableEnvironment와 LexicalEnvironment는 값의 변화가 실시간으로 반영되느냐 그렇지 않느냐의 차이만 있다.
- LexicalEnvironment

  - 실행 컨텍스트를 구성하는 환경 정보들을 모아 사전처럼 구성한 객체
  - 어휘적 환경, 사전적인 환경
  - 예를 들어 영한 사전은 한글로 설명해주는 내용들로 이루어진 사전인데, 이와 비슷하게 LexicalEnvironment는 어떠한 실행 컨텍스트 A에 대한 환경정보가 담겨있는 사전이다.
    - environmentRecord
      - 현재 컨텍스트 내부의 식별자 정보
      - 현재 문잭의 식별자 정보가 수집된다.
        - 수집 과정에서 발생하는 현상을 좀 더 쉽게 생각하는 방법이 있다.
          **현재 컨텍스트 식별자 정보들을 수집해서 environmentRecord에 담는 과정을 바로 “호이스팅” 이라고 한다.**
    - outerEnvironmentReference
      - 외부 환경(Lexical Environment)을 참조하는 정보들이 담겨있다.
      - 현재 문맥에 관련 있는 외부 식별자 정보를 참조한다.
  - 호이스팅

    - 현재 컨텍스트 식별자 정보들을 수집해서 environmentRecord에 담는 과정을 바로 “호이스팅” 이라고 했다.
    - 호이스팅의 뜻은 끌어올리다인데, 무엇을 끌어올리냐면 **식별자 정보를 실행 컨텍스트 맨 위로 끌어올린다.**
    - 호이스팅은 실제하는 현상이 아닌, environmentRecord의 정보수집 과정을 좀 더 쉽게 이해하기 위해서 만든 허구의 개념이다.
    - 호이스팅 예제1 (우리가 이해하기 쉽게 만들어 놓은 개념)

    ```jsx
    // 2-2-1
    console.log(a());
    console.log(b());
    console.log(c());

    function a() {
      return "a";
    }
    var b = function bb() {
      return "bb";
    };
    var c = function () {
      return "c";
    };
    ****
    ```

![](https://velog.velcdn.com/images/nu11/post/97395f9f-b6cb-4d90-a8c8-b1394b357b44/image.png)

    ```jsx
    //  {
    //    function a() { ... },
    //    b: undefined,
    //    c: undefined
    //  }
    function a() {
      return "a";
    }
    var b;
    var c;
    console.log(a());
    console.log(b());
    console.log(c());

    function a() {
      return "a";
    }
    var b = function bb() {
      return "bb";
    };
    var c = function () {
      return "c";
    };
    ```

    이렇게 위에 끌어올려진 내용 전체가 바로 environmentRecord다.
    실행 컨텍스트가 처음 생성되는 순간에 제일 먼저 하는 일이 이것이다. (정보수집)

    - 현재 컨텍스트에서 선언되어 있는 식별자들이 무엇이 있느냐라는 정보를 코드 순서대로 수집하다 보니 호이스팅한 것이랑 똑같은 개념이 되어 버린 것

- 스코프 체인

  - outerEnvironmentReference에 의해서 만들어진다.
  - 스코프란? 변수의 유효범위 ⇒ 변수의 유효범위는 실행 컨텍스트가 만드는 것
  - `inner` 에서 선언한 변수들은 `outer` 에서는 접근 할 수 없다. `outer` 에서 선언한 변수들은 `inner` 에서 접근 가능하다.
    - `inner` 함수의 environmentRecord는 오직 `inner` 안에서만 존재하기 때문.
    - `inner` 에서 `ouber` 접근은 outerEnvironmentReference를 통해서 접근 할 수 있다.
    - 즉, 외부로는 나갈 수 있는데 자신보다 더 안쪽으로는 접근할 수 없다라는게 유효범위!
  - 1. inner에서 어떤 변수를 찾으라고 명령을 하면, 일단 inner에서 먼저 찾는다.
       environmentRecord에서 해당 변수가 있는지 찾는다.

  2. inner에 없다면 outerEnvironmentReference를 타고 outer에 있는 LexicalEnvironment에서 environmentRecord에서 변수를 찾는다.
  3. 반복

  - **가장 가까운 자기 자신부터 점점 멀리 있는 스코프로 찾아 나간뒤 가장 먼저 찾아진 것만 접근할 수 있는 개념. 이것이 스코프 체인이다.**
  - 가장 먼저 찾아진 것만 접근할 수 있는 개념을 shadowing이라고 한다.
    ![](https://velog.velcdn.com/images/nu11/post/d0c5fb76-444e-4e5e-ad78-11af5789f3c7/image.png)

- 정리
  - Execution context는 함수를 실행할 때 필요한 환경정보를 담은 객체
    - 그 객체 안에는
      - Variable Environment
      - Lexical Environment
        - environmentRecord: 현재 문맥의 식별자(hoisting)
        - outerEnvironmentReference: 외부 식별자(scope chain)
      - this
        가 있다.
- 메모
  - 자바스크립트는 오직 함수에 의해서만 컨텍스트를 구분할 수 있다

---

## 자료형, 표기법, 배열 메소드

- 자바스크립트의 자료형
  - Primitive Type(원시 타입)
    - `let number = 12;`
    - 한 번에 하나의 값만 가질 수 있다
    - 하나의 고정된 저장 공간 이용
  - Non-Primitive Type(비 원시 타입)
    - `let array = [1, 2, 3, 4];`
    - 한 번에 여러 개의 값을 가질 수 있다
    - 여러 개의 고정되지 않은 동적 공간 사용
- 자바스크립트 표기법

  ```tsx
  let person = {
    key1: "value1", // 프로퍼티 (객체 프로퍼티)
    key2: "value2",
  }; // 객체 리터럴 방식

  console.log(person.key1); // 점 표기법
  console.log(person["key2"]); // 괄호 표기법
  ```

  - 점 표기법
    - `person.key1`
    - 장점: 간결하게 작성 가능, 가독성 측면에서 유리
  - 괄호 표기법
    - `person.["key2"]`
    - 장점: 객체의 프로퍼티에 변수를 활용하여 접근할 수 있음.
  - 점 표기법의 한계
    - `object.property = changingValue` 에러 발생
  - 점 표기법의 한계를 괄호 표기법으로 극복

    - `object[property] = changingValue` 프러퍼티에 접근 가능

    ```tsx
    let person = {
      key1: "value1", // 프로퍼티 (객체 프로퍼티)
      key2: "value2",
    }; // 객체 리터럴 방식

    console.log(person.key1); // 점 표기법
    console.log(person["key2"]); // 괄호 표기법
    console.log(getPropertyValue("key2"));

    function getPropertyValue(key) {
      return person[key];
    }
    ```

  - 비동기 데이터에서 괄호 표기법
    - 괄호 표기법을 통해 객체 내부가 정의되지 않은 경우에도 접근할 수 있는 이점이 있다.
    - 예를 들어 API 통신으로 어떤 객체(=result)를 받아올 때, 해당 객체의 내부 값을 변경하는 코드를 작성한다고 가정한다면 result 객체의 a 라는 프로퍼티가 존재해도, API 통신 이전에는 해당 프로퍼티는 정의되지 않은 상태다. 점 표기법으로는 정의되지 않은 result 객체에 접근할 수 없지만, 괄호 표기법은 해당 프로퍼티를 변수화하기 때문에 접근이 가능해진다.
    - 따라서 비동기로 데이터를 호출하는 경우, 프로퍼티에 접근하고 싶다면 괄호 표기법을 사용하자.

- `const person = {}` 처럼 let이 아닌 const를 쓰고 객체를 수정하면 에러가 발생할 것 같지만, 실제로는 발생하지 않는다. person의 object를 수정하는 것이지 person 자체를 수정하는 것이 아니기 때문이다.
  - person 자체를 수정한다는 것은 `person = {}` 와 같이 재정의 하는 것!
- 객체 안에 프로퍼티가 정의되어 있는지 확인하는 방법

  ```tsx
  let person = {
    key1: "value1", // 멤버
    key2: "value2", // 멤버
  };

  console.log(`key1: ${"key1" in person}`);
  console.log(`key3: ${"key3" in person}`);
  ```

- 반복문을 사용해서 객체 프로퍼티 접근

  ```tsx
  let person = {
    key1: "1",
    key2: "2",
  };

  const personKeys = Object.keys(person);
  const personValues = Object.values(person);

  for (let i = 0; i < personKeys.length; i++) {
    const curKey = personKeys[i];
    const curValue = person[curKey];

    console.log(`${curKey} : ${curValue}`);
  }

  for (let i = 0; i < personValues.length; i++) {
    console.log(personValues[i]);
  }
  ```

- 배열 내장 함수

  - forEach

    ```tsx
    const arr = [1, 2, 3, 4];
    const newArr = [];

    arr.forEach(function (elm) {
      newArr.push(elm * 2);
    });
    // (4) [2, 4, 6, 8]
    ```

  - map

    ```tsx
    const arr = [1, 2, 3, 4];
    const newArr = arr.map((elm) => {
      return elm * 2;
    });

    console.log(newArr);
    // (4) [2, 4, 6, 8]
    ```

  - includes

    ```tsx
    const arr = [1, 2, 3, 4];
    let number = 3;

    console.log(arr.includes(number));
    // true
    ```

  - indexOf

    ```tsx
    const arr = [1, 2, 3, 4];
    let number = 3;

    console.log(arr.indexOf(number));
    // 2
    ```

  - findIndex

    ```tsx
    const arr = [{ color: "red" }, { color: "blue" }, { color: "green" }];

    console.log(arr.findIndex((elm) => elm.color === "blue"));
    // 1
    ```

  - find

    ```tsx
    const arr = [{ color: "red" }, { color: "blue" }, { color: "green" }];

    const element = arr.find((elm) => elm.color === "blue");
    console.log(element);
    ```

  - filter

    ```tsx
    const arr = [
      { num: 1, color: "red" },
      { num: 2, color: "black" },
      { num: 3, color: "blue" },
    ];

    console.log(arr.filter((elm) => elm.color === "blue"));
    ```

  - slice

    ```tsx
    const arr = [
      { num: 1, color: "red" },
      { num: 2, color: "black" },
      { num: 3, color: "blue" },
    ];

    console.log(arr.slice(0, 2));
    ```

  - concat

    ```tsx
    const arr1 = [
      { num: 1, color: "red" },
      { num: 2, color: "black" },
      { num: 3, color: "blue" },
    ];

    const arr2 = [
      { num: 4, color: "green" },
      { num: 5, color: "blue" },
    ];

    console.log(arr1.concat(arr2));
    ```

  - sort (원본 배열의 데이터를 정렬)

    ```tsx
    let chars = ["나", "다", "가"];

    chars.sort();

    console.log(chars);
    ```

  - sort(숫자형 오름차순 정렬)

    ```tsx
    let numbers = [0, 1, 2, 10, 30, 20, 3];

    const compare = (a, b) => {
      // 1. 같다
      // 2. 크다
      // 3. 작다

      if (a > b) {
        // 크다
        return 1;
      }
      if (a < b) {
        // 작다
        return -1;
      }

      // 같다
      return 0;
    };

    numbers.sort(compare);

    console.log(numbers);
    ```

  - sort(숫자형 내림차순 정렬)

    ```tsx
    let numbers = [0, 1, 2, 10, 30, 20, 3];

    const compare = (a, b) => {
      // 1. 같다
      // 2. 크다
      // 3. 작다

      if (a > b) {
        // 크다
        return -1;
      }
      if (a < b) {
        // 작다
        return 1;
      }

      // 같다
      return 0;
    };

    numbers.sort(compare);

    console.log(numbers);
    ```

  - join(문자열 하나로 합쳐짐)

---

## Truthy & Falsy

- Truthy
  - 참 같은 값
    ```jsx
    // Truthy 값
    if (true)
    if ({})
    if ([])
    if (42)
    if ("0")
    if ("false")
    if (new Date())
    if (-42)
    if (12n)
    if (3.14)
    if (-3.14)
    if (Infinity)
    if (-Infinity)
    ```
- Falsy

  - 거짓같은 값
    ```jsx
    if (false)
    if (null)
    if (undefined)
    if (0)
    if (-0)
    if (0n)
    if (NaN)
    if ("")
    ```
  - 논리 AND 연산자(&&)
    - 첫 번째 객체가 거짓 같은 값이라면, 해당 객체를 반환한다.
    - `false && "dog"` ⇒ false
    - `0 && "dog"` ⇒ 0

- undefined는 객체가 아니기 때문에 프로퍼티에 접근할 수 없다.

  ```jsx
  const getName = (person) => {
    return person.name; // TypeError: Cannot read properties of undefined (reading 'name')
  };

  let person;
  const name = getName(person);
  console.log(name);
  ```

  ```jsx
  const getName = (person) => {
    if (person === undefined) {
      return "객체가 아닙니다"; // 실행
    }
    return person.name; // 실행X
  };

  let person;
  const name = getName(person);
  console.log(name); // 객체가 아닙니다
  ```

  ```jsx
  // null 예외처리까지 해주기에는 타이핑 양이 많아져 번거롭다.
  const getName = (person) => {
    if (person === undefined || person === null) {
      return "객체가 아닙니다";
    }
    return person.name;
  };

  let person = null;
  const name = getName(person);
  console.log(name);
  ```

  ```jsx
  // not 연산자를 붙이면 해결!
  const getName = (person) => {
    if (!person) {
      // false에 not을 붙이면 => true가 되기 때문에 조건문 실행
      return "객체가 아닙니다";
    }
    return person.name;
  };

  let person = null;
  const name = getName(person);
  console.log(name);
  ```

---

## 데이터 타입

- 스택 메모리
  - 변수
  - 기본형 데이터
  - 정적 할당
- 힙 메모리
  - 참조형 데이터
  - 동적 할당
- 값을 직접 저장
  - 데이터 할당시에는 빠름
  - 비교에 비용이 많이 든다
  - 메모리 낭비가 심하다
- 값의 주소를 저장
  - 데이터 할당시에는 느림
  - 비교에 비용이 들지 않음
    - 같은 값이 (전체 메모리 공간상에)오직 하나만 존재!!!
    - 불변값
  - 메모리 낭비 최소화
- 메모
  - 기본형의 값을 바꿨을 때는 바로 바뀐다
  - 참조형에 있는 값을 바꿨을 때는 여전히 똑같은 객체를 바라보고 있다
  - 메모리 구조 공간에는 값이 하나씩 밖에 못 들어간다

---
