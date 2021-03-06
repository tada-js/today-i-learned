# JavaScript 학습 및 복습

<br>

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

<br>

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

<br>

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

  <br>

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

<br>

## 비동기, 동기

- 자바스크립트 싱글 스레드

  - 예제1(작업 수행 방식)

  ```jsx
  function taskA() {
    console.log("TASK A");
  }

  function taskB() {
    console.log("TASK B");
  }

  function taskC() {
    console.log("TASK C");
  }

  taskA();
  taskB();
  taskC();
  ```

  - 동기 방식의 처리
    - |Thread| → |taskA| → |taskB| → |taskC|
      0.3초 0.5초 0.2초
    - 자바스크립트는 코드가 작성된 순서대로 작업을 처리함
    - 이전 작업이 진행 중 일 때는 다음 작업을 수행하지 않고 기다림 ⇒ 블로킹 방식 (taskA가 끝나야 taskB를 실행)
    - 먼저 작성된 코드를 먼저 다 실행하고 나서 뒤에 작성된 코드를 실행한다.
    - 스레드는 코드를 한 줄 한 줄 실행시켜 주는 친구(일꾼)
  - 동기처리 방식의 문제점
    - |Thread| → |taskA| → |taskB| → |taskC|
      0.3초 20초 10초
    - 동기적 처리의 단점은 하나의 작업이 너무 오래 걸리게 될 시, 모든 작업이 오래 걸리는 하나의 작업이 종료되기 전 까지 올 스탑 되기 때문에, 전반적인 흐름이 느려진다.
    - 웹사이트에서 버튼 하나를 눌렀는데 처리가 30초 씩 걸리면? 속 터진다.
  - 멀티 쓰레드였다면?

    - |Thread| → |taskA|
      0.3초
    - |ThreadA| → |taskB|
      20초
    - |ThreadB| → |taskC|
      10초
    - 코드를 실행하는 친구(일꾼) Thread를 여러 개 사용하는 방식인 멀티쓰레드(MultiThread) 방식으로 작동시키면 위와 같이 작업 분할 가능
    - 오래 걸리는 일이 있어도 다른 일꾼 Thread에게 지시하면 되므로 괜찮음
    - 하지만 자바스크립트는 싱글 쓰레드로 동작함
    - 여러 친구(일꾼)을 여러 개 사용하는 방법은 사용 불가

  - 비동기 작업(싱글 쓰레드 방식의 동기처리 문제점 극복)
    - |Thread| → |taskA| → …
      |taskB| 20초
      |taskC| 30초
    - 싱글 쓰레드 방식을 이용하면서, 동기적 작업의 단점을 극복하기 위해 여러 개의 작업을 동시에 실행시킨다.
    - 먼저 작성된 코드의 결과를 기다리지 않고 다음 코드를 바로 실행함 ⇒ 논 블로킹 방식
      그렇다면 작업을 동시에 실행시키는 비동기는 결과가 정상적으로 끝났는지, 결과는 나왔는지 어떻게 확인할 수 있을까?
      그럴 때는 비동기로 실행된 A, B, C에게 작업이 끝나면 콜백 함수를 실행시키게 하면 된다.
  - 콜백함수 사용
    - 예제1
      ```jsx
      taskA((resultA) => {
        console.log(`A 끝. 작업 결과 : ${resultA}`); // A 콜백
      });
      taskB((resultB) => {
        console.log(`A 끝. 작업 결과 : ${resultB}`); // B 콜백
      });
      taskC((resultC) => {
        console.log(`A 끝. 작업 결과 : ${resultC}`); // C 콜백
      });
      ```
    - |Thread| → |taskA, A 콜백| → …
      |taskB, B 콜백| 20초
      |taskC, C 콜백| 10초
    - 이렇듯, 비동기 처리할 때는 콜백함수를 붙여서 그 비동기 처리 결과값이나, 결과가 정상적으로 끝났는지 확인한다.
      이제 콜백함수를 직접 만들어보자.
  - 예제2

    ```jsx
    function taskA() {
      console.log("A 작업 끝");
    }

    taskA();
    console.log("코드 끝");
    ```

    지금까지의 동기적 방식으로는 taskA의 함수가 종료되기 전까지는 `console.log`는 실행할 수 없다. 이제 위의 예제를 비동기 방식으로 바꿔보자.

    ```jsx
    function taskA() {
      setTimeout(() => {
        console.log("A TASK END");
      }, 2000);
    }

    taskA();
    console.log("코드 끝");
    ```

    ![](https://velog.velcdn.com/images/nu11/post/4b58321b-99d3-4bbe-836f-7a8add2e0002/image.png)

    `setTimeout` 내장 비동기 함수를 통해 `console.log("코드 끝")` 이 먼저 출력되고, 이후에 `taskA` 함수의 `console.log("A TASK END")` 가 출력된 것을 확인할 수 있다.
    즉, 지시 순서는 `taskA() -> console.log("코드 끝")` 이지만 `taskA()` 가 끝날때 까지 기다리지 않고 코드 끝이 출력된 것이다.

    ```jsx
    function taskA(a, b, cb) {
      setTimeout(() => {
        const res = a + b;
        cb(res);
      }, 3000);
    }

    function taskB(a, cb) {
      setTimeout(() => {
        const res = a * 2;
        cb(res);
      }, 1000);
    }

    taskA(3, 4, (res) => {
      console.log("A TASK RESULT : ", res);
    });

    taskB(7, (res) => {
      console.log("B TASK RESULT : ", res);
    });

    console.log("코드 끝");
    ```

    ![](https://velog.velcdn.com/images/nu11/post/9ea5b825-56a9-4047-abee-a4aa4f3d0fb0/image.png)

    마찬가지로 `taskA()` 가 먼저 호출 되었지만 `taskB()` 가 먼저 출력된 이유는 `taskB` 는 1초 기다리고, `taskA`는 3초 기다리기 때문!

<br>

## this

- 호출하는 방식에 따른 this

  - 전역공간에서 this ⇒ 전역 객체를 가리킨다.
    - window / global ⇒ 자바스크립트가 실행되는 환경에 따라서 전역객체의 정보가 달라진다. 브라우저는 window, node.js는 global
  - 함수 호출시 this ⇒ 전역 객체를 가리킨다.
    - window / global ⇒ 이상하다는 의견이 분분하나, 무조건 언제나 전역 객체를 가리킨다.
  - 메서드로 호출시 this ⇒ 메서드 호출한 주체(메서드명 앞)

    ```jsx
    var a = {
      b: function () {
        console.log(this);
      },
    };
    a.b();
    ```

    `a.b()` 에서 `.` 앞에 있는 `a` 가 `this`가 된다. 그리고 b함수를 a객체의 메서드로서 호출했다. `person['info'].getName()` 같은 대괄호 표기법도 마찬가지다. `person['info']` 까지가 this, `getName()`은 메소드명이 되겠다.

    - 메서드 내부함수에서의 우회법

      ```jsx
      var a = 10;
      var obj = {
        a: 20,
        b: function () {
          console.log(this.a); // 20

          function c() {
            console.log(this.a); // 10
          }
          c();
        },
      };
      obj.b();
      ```

      - 전역 객체의 프로퍼티 a를 달라고 했더니 전역 변수 a를 주는 것이다.
        즉, window.a ⇒ 전역변수.
        전역 객체와 전역 변수는 별개의 개념이어야 되는데 실제로는 전역 변수가 곧 전역 객체의 프로퍼티로 동작 하더라는 것이다. 이것도 자바스크립트 설계상 오류라는 지적이 많다.
      - 그럼 이때 this가 obj를 바라보게끔 하려면 어떻게 해야될까? (this === obj)
        아쉽게도 call, apply 같은 명시적인 this 바인딩 명령을 쓰지 않고는 this 자체를 직접 다른 값으로 덮어씌울 수는 없다.
        ES5 call / apply ⇒ `c.call(this)` , `c.apply(this)`
      - 스코프 체인을 이용해서 방법을 찾아보자.

        ```jsx
        var a = 10;
        var obj = {
          a: 20,
          b: function () {
            var self = this;
            console.log(this.a); // 20

            function c() {
              console.log(self.a); // 20
            }
            c();
          },
        };
        obj.b();
        ```

        - 내부 함수는 자신의 LexicalEnvironment에서 self를 찾는데, 없으니까 outerEnvironmentReference를 타고 외부에 있는 b 함수의 LexicalEnvironment에서 self를 찾는다. 그 self에는 앞서 들어온 this가 담겨 있을 것이고, 이때의 this는 obj.b를 호출할 때의 this니까 obj를 가리키게 되는 것이다.
        - 하지만 ES6에서는 this를 바인딩 하지 않는 arrow function이 등장하게 되면서 이러한 우회법을 쓸 필요성이 사라졌다. this를 바인딩 하지 않으니까 스코프 체이닝 상의 this에 바로 접근 할 수 있게 된다.

      - ES6 arrow function

        ```jsx
        var a = 10;
        var obj = {
          a: 20,
          b: function () {
            var self = this;
            console.log(this.a); // 20

            const c = () => {
              console.log(this.a); // 20
            };
            c();
          },
        };
        obj.b();
        ```

        - arrow function은 this를 바인딩 하지 않으니까 상위에 있는 this를 그대로 쓴다.

  - 콜백 호출시 ⇒ 기본적으로는 함수 내부에서와 동일

- 메모
  - ThisBinding은 실행컨텍스트 활성화될 때 한다.
    - 즉, this가 함수가 호출될 때 비로소 결정되는 것이다.
    - 호출하는 방식에 따라 다르다 ⇒ 동적으로 바인딩 된다.
    - 오브젝트 내 함수안에서 쓰면 그 함수를 가지고 있는 오브젝트를 뜻한다.

<br>

## 브라우저

- 브라우저는 자바스크립트 실행해주는 친구들
- 웹브라우저 내부
  - 스택(Stack)이라는 공간이 있는데 거기에 코드를 한 줄씩 실행한다.
    스택에 담겨 있는 코드 `console.log(i)` ⇒ `i` 변수가 어딨지? ⇒ 힙(Heap)에 저장되어 있는 변수 찾기 - 스택은 코드 실행해주는 곳인데 하나 뿐이기 때문에 코드를 한 줄씩 실행한다. ⇒ 자바스크립트는 single threaded - `setTimeout()` 함수는 바로 실행할 수 없는 코드이기 때문에(n초 뒤에 실행되기 때문) 대기실로 보낸다. 대기실로 보내는 코드들은 “Ajax 요청 코드, 이벤트리스너, setTimeout” 등이 있다. - 대기실에서 다시 스택으로 옮기는데 바로 스택으로 옮기지 않고 Queue라는 곳으로 보낸다. 대기 끝난 코드가 Queue에 차례로 쌓이게 되고, 다시 하나씩 스택으로 옮겨진다. **단, 스택이 비어있을 때만 올려보낸다.**
    왜 대기실에서 스택으로 바로 옮기지 않고 Queue라는 곳을 거치냐면, 스택은 매우 바쁜 공간이기 때문이다. - 스택에서 처리하는데 엄청 오래 걸리는 코드가 있다면 대기실에 있는 코드들은 계속 대기중이기 때문에 작동하지 않는다. 예를 들어, 버튼을 클릭하면 모달창이 출력되는 이벤트 리스너가 있다면 엄청 오래 걸리는 코드가 스택에서 실행이 끝날 때까지 작동하지 않는다. ⇒ 브라우저 프리징 원인이 되기도 한다. - 교훈 1. stack을 바쁘게 하지말자. 2. queue를 바쁘게 하지말자.
  - 힙(Heap)
    `i = {name: “Kim”}`

<br>

## 동기 / 비동기

- 자바스크립트는 동기식처리(Synchronous)
  - 한 번에 코드 한 줄씩 차례로 실행
- 비동기식처리(Asynchronous)
  - 오래걸리는 작업이 있으면 제껴두고 다른거부터 처리하는 방식 ⇒ 자바스크립트가 아닌 자바스크립트를 실행하는 브라우저 덕분에 가능
    - `setTimeout()` ⇒ 비동기식으로 도와주는 함수
      - Web API에 오래걸리거는 함수(setTimeout, 이벤트리스너)들을 보낸다.
      - Web API 덕분에 오래걸리는 작업이 있으면 제껴두고 다른거부터 처리하는 비동기식 처리가능
- 콜백함수

  - 자바스크립트를 순차적으로 실행하려면 콜백함수를 사용함
    - 콜백함수란 함수안에 들어가는 함수
  - 콜백함수를 이용한 함수 디자인

    ```jsx
    function 첫째함수() {}

    function 둘째함수() {}

    첫째함수();
    둘째함수();
    ```

    첫째함수 다음에 둘째 함수를 실행하고 싶다면?

    ```jsx
    function 첫째함수(매개변수) {
      매개변수();
    }

    function 둘째함수() {}

    첫째함수(둘째함수);
    ```

    콜백함수는 동기, 비동기 개념이 아닌 함수 디자인 패턴일 뿐이다.

- Promise 패턴

  - 콜백함수의 문제점 때문에 더 쉽게 쓰기 위한 패턴

  ```jsx
  // 콜백함수
  첫째함수(function() {
    둘째함수(function() {
      셋째함수(function() {
      }
    }
  }

  // Promise
  첫째함수().then(function() {
  }).then(function() {
  })
  ```

- Promise

```jsx
var 프로미스 = new Promise();

프로미스.then(function () {
  // 프로미스가 성공일 경우 실행할 코드
});
```

- 콜백함수 만드는거랑 비슷한데 콜백함수 보다는 약간 기능이 많다.
- `catch` 를 통해 실패할 경우에 대해서도 코드 실행이 가능하다.
  - 일반 콜백함수 ⇒ 1번 실행 후 2번 실행해주세요
  - Promise ⇒ 1번 실행 후, 성공시 2번 실행해주세요~ 실패하면 3번 실행해 주세요~

```jsx
// 성공
var 프로미스 = new Promise(function (성공, 실패) {
  var 어려운연산 = 1 + 1;
  성공();
});

프로미스
  .then(function () {
    // 성공 출력
    console.log("성공");
  })
  .catch(function () {
    console.log("실패");
  });

// 실패
var 프로미스 = new Promise(function (성공, 실패) {
  var 어려운연산 = 1 + 1;
  실패();
});

프로미스
  .then(function () {
    console.log("성공");
  })
  .catch(function () {
    // 실패 출력
    console.log("실패");
  });
```

- Promise의 3가지 상태
  - 성공 ⇒ resolved
  - 판정 대기중 ⇒ pending
  - 실패 ⇒ rejected

<br>

## 전개 연산자

ECMAScript6(2015)에서 새로 추가된 전개 연산자(Spread Operator)란 객체나 배열의 값을 하나 하나 넘기는 용도로 사용할 수 있다. 전재 연산자를 사용하는 방법은 점 세개 `...` 를 붙이면 된다.

> 예제1

```jsx
const fruits = ["Apple", "Banana", "Cherry"];
console.log(fruits);
console.log(...fruits);
// console.log('Apple', 'Banana', 'Cherry')

function toObject(a, b, c) {
  return {
    a: a,
    b: b,
    c: c,
  };
}
console.log(toObject(...fruits));
```

![](https://velog.velcdn.com/images/nu11/post/27f4468b-3fed-419d-8a09-37424516d0d2/image.png)

`console.log(...fruits)` 와 같이 작성한 것을 전개 연산자 라고 한다. 이는 `console.log('Apple', 'Banana', 'Cherry')` 와 같은 출력 결과를 나타낸다.

그렇다면 전개 연산자를 사용해서 어떻게 활용할 수 있는지 아래의 `예제2` 를 통해서 살펴보자.

> 예제2

```jsx
const fruits = ["Apple", "Banana", "Cherry"];

function toObject(a, b, c) {
  return {
    a: a,
    b: b,
    c: c,
  };
}
console.log(toObject(...fruits));
```

![](https://velog.velcdn.com/images/nu11/post/ff04d549-0b4b-4634-a484-e7e02ce89c56/image.png)

함수 `toObject` 는 객체 데이터로 변환해주는 함수라는 뜻을 가지고 있는데 매개변수로는 `a, b, c` 3개가 있다. 그리고 그 매개변수를 `toobject` 함수 내부 로직에서 사용하고 있다. 내부 로직으로는`return` 키워드를 통해서 어떤 객체 데이터를 반환하고 있다. 객체 데이터는 `a: a` `b: b` `c: c` 라는 속성을 매개변수에서 받아오고 있다. 그리고 `console.log(toObject(...fruits))` 로 `toObject` 함수를 실행하고 있는데 `...` 전개 연산자를 사용해서 `fruits` 배열 데이터를 전개하고 있다. 따라서 쉼표로 구분되어져 있는 각각의 아이템으로 전개돼서 들어가진다. 매개변수 `a` 에는 `'Apple'`, `b` 에는 `'Banana'` , `c` 에는 `'Cherry'` 가 인수로 들어가게 된다.

> 예제3

```jsx
console.log(toObject(...fruits));
// expected output: {a: 'Apple', b: 'Banana', c: 'Cherry'}
console.log(toObject(fruits[0], fruits[1], fruits[2]));
// expected output: {a: 'Apple', b: 'Banana', c: 'Cherry'}
```

만약 전개 연산자를 사용하지 않는다면?

`console.log(toObject(fruits[0], fruits[1], fruits[2]))` 처럼 하나씩 수동으로 인덱스 번호를 넣어줘야 된다. 하지만 아이템 개수가 3개가 아닌 10개, 100개, 1000개로 늘어나게 된다면? 모두 수동으로 인덱스 번호를 넣어줘야 되기 때문에 비효율적이다.

> 예제4

```jsx
const fruits = ["Apple", "Banana", "Cherry", "Orange"];

function toObject(a, b, c) {
  return {
    a: a,
    b: b,
    c: c,
  };
}
console.log(toObject(...fruits));
```

`fruits` 배열 데이터에 `Orange` 아이템을 하나 추가하면 아이템의 개수는 총 4개가 된다. 하지만 `toObject` 함수의 매개변수는 `a, b, c` 총 3개이므로 `Orange` 를 받아줄 매개변수는 없는 상태다. 따라서 `Orange` 는 출력하지 못한다.

![](https://velog.velcdn.com/images/nu11/post/7d6b3826-4511-4445-9f2a-d57085aa3c37/image.png)

> 예제5 (rest parameter)

```jsx
const fruits = ["Apple", "Banana", "Cherry", "Orange"];

function toObject(a, b, ...c) {
  return {
    a: a,
    b: b,
    c: c,
  };
}
console.log(toObject(...fruits));
```

![](https://velog.velcdn.com/images/nu11/post/7d004c04-97cc-4d50-b55d-5f1a7824363a/image.png)

위의 예제처럼 `function toObject(a, b, ...c)` 와 같이 작성하는 것을 **rest parameter(나머지 매개변수)**라고 부른다. `'Apple'` 은 `a`

`'Banana'` 는 `b` 에 들어가게 되고 그 외 나머지 `'Cherry'` 와 `'Orange'` 는 모두 `c` 에 들어가게 된다.

> 예제6 (축약형으로 정리)

```jsx
const fruits = ["Apple", "Banana", "Cherry", "Orange"];

function toObject(a, b, ...c) {
  return { a, b, c };
}
console.log(toObject(...fruits));
```

속성의 이름 `a:` 과 데이터의 이름 `a` 이 같으면 축약형으로 하나만 작성해도 된다.

> 예제7(화살표 함수로 정리)

```jsx
const fruits = ["Apple", "Banana", "Cherry", "Orange"];

const toObject = (a, b, ...c) => ({ a, b, c });

console.log(toObject(...fruits));
```

객체 데이터를 사용할 때는 중괄호 `{}`를 사용하는데 중괄호는 화살표 함수 `=>` 에서 함수의 범위를 나타내는 블럭으로 해석이 된다. 그렇기 때문에 소괄호 `()` 를 사용해서 객체 데이터를 정의해주자. `({a, b, c})`

<br>

## Arrow function

- Arrow function 장점

  - 입출력 기계 만들 때 보기 쉽다.

  ```jsx
  const 함수 = (a) => {
    return a + 10;
  };
  ```

  - 파라미터가 1개면 소괄호 생략이 가능하다.
  - 코드가 한 줄이면 중괄호도 생략이 가능하다.

- Arrow function 예시

  - forEach 콜백함수

  ```jsx
  // array 자료 개수만큼 내부 코드 실행. a는 array 내의 자료들
  [1, 2, 3, 4].forEach(function (a) {
    console.log(a);
  });

  // Arrow function 사용
  [1, 2, 3, 4].forEach((a) => console.log(a));
  ```

  - 이벤트 리스너

  ```jsx
  document.getElementById("버튼").addEventListener("click", (e) => {
    this;
  });
  ```

  바깥에 있던 this값을 내부에서 그대로 사용.
  일반 이벤트 리스너에선 `this == e.currentTarget`이지만 arrow function에서는 `this == 바깥의 this값`

- Object 안의 함수

```jsx
const 오브젝트 = {
  함수: () => {
    console.log("Arrow Function");
  },
};
오브젝트.함수();
```

<br>

## 가져오기, 내보내기 import export

> 예제1 (import)

```jsx
import _ from "lodash"; // From `node_modules` !
import getType from "./getType"; // getType.js
import getRandom from "./getRandom"; // getRandom.js

console.log(_.camelCase("the hello world"));
console.log(getType([1, 2, 3]));
console.log(getRandom(), getRandom());
```

`import _ from 'lodash'` 코드는 `'lodash'` 라는 패키지를 `install` 해서 그것을 `import` 키워드를 통해 `'node modules'` 폴더에 가지고 와서 `_` 기호로 활용을 하겠다는 선언이다.

> 예제2 (.js 파일 export)

```jsx
export default function getType(data) {
  return Object.prototype.toString.call(data).slice(8, -1);
}
```

`.js` 자바스크립트 파일은 외부에 있는 `.js` 파일을 `import` 키워드를 통해 가지고 올 수도 있거나 그 파일 내부에서 특정한 내용을 `export` 키워드를 통해 내보낼 수도 있다.

<br>

### 그림으로 이해하기

![](https://velog.velcdn.com/images/nu11/post/ee4f6d5a-592b-4d4a-829d-bba627fbc5ac/image.jpg)

1. 그림에는 `module.js` 파일이 존재한다.
2. 자바스크립트 파일은 기본적으로 외부에 있는 다른 자바스크립트 파일을 가지고 올 수 있는 **하나의 통로**를 가지고 있다. 대표적으로 `import` 키워드를 통해서 외부에 있는 다른 자바스크립트 파일을 가지고 올 수 있다.
3. 그리고 자바스크립트 파일은 그 안에 있는 특정 내용을 내보낼 수 있는 두 개의 통로를 가지고 있다.
   `Default export` : 이름을 따로 지정하지 않아도 되는 기본 통로다. `default` 키워드를 통해 사용할 수 있다.
   `Named export` : 이름을 지정해야 되는 통로

<br>

### default export

> 예제1 (getType.js)

```jsx
export default function getType(data) {
  return Object.prototype.toString.call(data).slice(8, -1);
}
```

> 예제2(main.js)

```jsx
import _ from "lodash"; // From `node_modules` !
import getType from "./getType"; // getType.js
import getRandom from "./getRandom"; // getRandom.js

console.log(_.camelCase("the hello world"));
console.log(getType([1, 2, 3]));
console.log(getRandom(), getRandom());
```

1.  `export` 키워드를 통해 내보내기를 할 건데 기본통로인 `Default export` 를 사용할 것이다. 따라서 뒤에 `default` 키워드를 붙여준다.
2.  `function` 참조형 데이터 함수를 내보내기 할 건데 함수명이 `getType` 으로 지정이 되어져 있지만 `default` 기본 통로로 내보내기를 할 때는 함수명을 지정해주지 않아도 된다. 따라서 `getType` 을 지워도 정상적으로 작동하여 출력하는 것을 확인할 수 있다.
3.  마찬가지로 기본통로를 빠져나가는 데이터는 따로 이름을 지정하는 것이 아니기 때문에 `main.js` 에서 불러올 때도 굳이 `getType` 으로 작성을 안해도 되고 내가 원하는 이름으로 지정할 수 있다. 예를 들어 `checkType` 으로 변경하고 싶다면 `import checkType` 으로 바꿔도 무방하다. 다만, `from './getType'` 상대경로는 정확하게 작성해야 된다.
4.  즉, 기본통로 `default` 로 내보내지는 데이터는 이름을 지정하지 않아도 되고 `import` 로 가져올 때는 이름을 수정해도 문제 없다.

<br>

### named export

> 예제1 (getRandom.js)

```jsx
// getRandom.js
export function random() {
  return Math.floor(Math.random() * 10);
}
```

> 예제2 (main.js)

```jsx
// main.js
import _ from "lodash"; // From `node_modules` !
import getType from "./getType"; // getType.js
import { random } from "./getRandom"; // getRandom.js

console.log(_.camelCase("the hello world"));
console.log(getType([1, 2, 3]));
console.log(random(), random());
```

1. `default` 키워드를 사용할 때는 기본 통로로 나오기 때문에 별도의 기호 없이 `import getType` 처럼 작성해도 되지만 이름이 지정된 `named export` 같은 경우에는 `{}` 중괄호 기호를 사용해서 `import { random }` 으로 사용해줘야 된다.
2. 여기서 기본 통로와 이름을 지정해줘야 되는 통로의 가장 큰 차이점이 나타난다. **기본 통로 `default export` 는 하나의 데이터만 내보낼 수 있지만 이름이 지정된 통로 `named export` 는 여러개의 데이터도 내보낼 수 있다.**

> 예제3 (named export getRandom.js)

```jsx
// getRandom.js
export function random() {
  return Math.floor(Math.random() * 10);
}
export const user = {
  name: "test",
  age: 85,
};
```

> 예제4(main.js)

```jsx
// main.js
import _ from "lodash"; // From `node_modules` !
import getType from "./getType"; // getType.js
import { random, user } from "./getRandom"; // getRandom.js

console.log(_.camelCase("the hello world"));
console.log(getType([1, 2, 3]));
console.log(random(), random());
console.log(user);
```

![](https://velog.velcdn.com/images/nu11/post/19f336da-577d-4b8b-9c50-2a499419577e/image.jpg)

`named export` 는 여러개의 데이터를 내보낼 수 있기 때문에 정상적으로 출력된다.

하지만 기본통로인 `default export` 는 어떨까?

> 예제5(default export /getType.js)

```jsx
// getType.js
export default function getType(data) {
    return Object.prototype.toString.call(data).slice(8, -1)
}
export default 123
```

![](https://velog.velcdn.com/images/nu11/post/67292845-716e-444d-b977-298f844ea90a/image.jpg)

기본통로인 `default export` 로 `getType.js` 하나의 파일에서 내보내는 데이터가 2개가 됐더니 `error` 가 발생했다. `error` 를 읽어보면 `Only one default export allowed per module.` 라고 친절하게 설명해준다. 즉, 하나의 모듈에서 하나의 기본 내보내기만 가능하다는 것이다.

<aside>
💡 자바스크립트 파일에서 하나의 데이터만 내보낼 때는 `default export` 를 사용하고 여러개의 데이터를 내보낼 때는 `named export` 를 사용하자.

</aside>

> 예제6 (\* as export /getRandom.js)

```jsx
// getRandom.js
import _ from "lodash"; // From `node_modules` !
import getType from "./getType"; // getType.js
// import { random, user } from './getRandom' // getRandom.js
import * as R from "./getRandom";

console.log(_.camelCase("the hello world"));
console.log(getType([1, 2, 3]));
// console.log(random(), random())
console.log(R);
```

1. `getRandom.js` 에서 모든 데이터를 한 번에 가져오고 싶다면 어떻게 해야될까?
2. 애스터리스크 `*` 와 `as` 를 작성하면 된다. `import * as 원하는이름` 으로 작성하자.
3. 애스터리스크 `*` 를 보통 와일드 카드라고 부른다.

- 와일드카드(Wildcard Character, \*) : 여러 내용을 한꺼번에 지정할 목적으로 사용하는 기호를 가리킨다.

> 예제7 (named export와 default export 혼용 /getRandom.js)

```jsx
// getRandom.js
export function random() {
  return Math.floor(Math.random() * 10);
}
export const user = {
  name: "test",
  age: 85,
};
export default 123;
```

![](https://velog.velcdn.com/images/nu11/post/72f6ab66-0be3-4afc-8a3e-b9222287980e/image.jpg)

1. `getRandom.js` 파일 내용으로는 하나의 데이터만 내보낼 수 있는 기본 통로인 `default export` 와 여러개의 데이터를 내보낼 수 있는 `named export` 가 같이 작성되어져 있다.
2. 혼용해서 작성되었음에도 결과를 보면 문제 없음을 알 수 있다.
3. 결국에는 하나의 자바스크립트 파일에서 두 개의 `export` 를 모두 사용할 수 있기 때문에 굳이 하나만 고집할 필요는 없다.

<br>

## JSON

### JSON(제이슨, JavaScript Object Notation)

- 자바스크립트 데이터를 표현하는 하나의 포맷.
- 속성-값 쌍. 혹은 key-value
- 사람이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷.
- 비동기 브라우저/서버 통신에서 사용됨.
- 예전에 많이 사용하던 `XML` 을 대체하는 주요 데이터 포맷.
- 인터넷에서 데이터를 주고 받을 때 그 데이터를 표현하는 방법.
- 컴퓨터 프로그램의 변수값을 표현하는데 적합하다.
- `JSON` 의 공식 인터넷 미디어 타입은 `application/json` 이며, `JSON` 의 파일 확장자는 `.json` 이다.

<br>

### JSON 자료형

- 수(Number)
- 문자열(String): **문자열은 큰 따옴표 `"` 로 구분**하며 역슬래시 이스케이프 문법을 지원한다.
- 참/거짓(Boolean): `true` 또는 `false` 값
- 배열(Array): 순서가 있는 리스트이며, 대괄호로 나타내고 요소는 쉼표로 구분한다.
- 객체(Object): 순서가 없는 이름/값 쌍의 집합으로 이름(키)이 문자열이다.
- `null`: 빈 값으로, `null`을 사용

<br>

- package.json

  ```jsx
  {
    "name": "js-test",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "dev": "parcel index.html",
      "build": "parcel build index.html"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
      "lodash": "^4.17.21",
      "parcel-bundler": "^1.12.5"
    }
  }
  ```

`node.js` 환경에서 `npm` 을 통해서 패키지를 관리하는 프로젝트는 기본적으로 `package.json` 파일이 자동으로 해석이 돼서 프로젝트를 구성할 수 있는 구조가 된다. 그래서 터미널에 `npm init -y` 를 입력하면 자동으로 `package.json` 파일이 만들어진다. 결국 `node.js` 환경에 `npm` 은 `package` 라는 이름에 `.json` 파일로 환경에 구성 옵션을 만들어서 관리하는 것이고 `json` 이라는 것은 하나의 데이터처럼 취급할 수 있기 때문에 구성 옵션을 관리하기가 좋은 `json` 이라는 포맷처럼 관리를 한다는 것이다.

<br>

- 예제1

  ```jsx
  const user = {
    name: "userName",
    age: 85,
    emails: ["userEmail@gmail.com", "userEmailTwo@gmail.com"],
  };
  console.log("user", user);

  const str = JSON.stringify(user);
  console.log("str", str);
  console.log(typeof str);

  const obj = JSON.parse(str);
  console.log("obj", obj);
  ```

![](https://velog.velcdn.com/images/nu11/post/16838dcc-f615-4b81-9e31-15b120109256/image.jpg)

<br>

- 예제2 (myData.json)

  ```jsx
  {
    "string": "MyName",
    "number": 123,
    "boolean": true,
    "null": null,
    "object": {},
    "array": []
  }
  ```

  `예제2` 를 통해 확인할 수 있듯이 `.json` 파일은 `json` 포맷 안에서 사용할 수 있는 기본 데이터(문자, 숫자..)들을 그대로 명시할 수 있다.

`undefined` 데이터는 사용 불가능!
`.json` 확장자를 가지고 있는 하나의 파일은 곧 하나의 데이터를 의미한다. 따라서 한 개의 `.json` 파일 안에는 한 개의 데이터만 명시 가능하다. `123 [] false` 처럼 두 개 이상의 데이터는 명시하지 못한다.
예제의 `myData.json` 파일의 경우에는 객체 데이터 `{}` 하나만 명시되어져 있다.
그렇다면 `myData.json` 파일을 어떻게 활용하는지 `main.js` 파일로 이동해서 알아보자.

<br>

- 예제3 (main.js)

  ```jsx
  import myData from "./myData.json";

  console.log(myData);
  ```

  ![](https://velog.velcdn.com/images/nu11/post/e0b00cf1-d052-41b8-a065-ce80e4397994/image.jpg)

  `.json` 이라는 확장자를 가진 파일은 사실 **하나의 문자 데이터**이다. 객체 데이터처럼 출력이 됐다고 객체 데이터라고 착각하면 안된다. `import` 를 통해 `.json` 이라는 **문자 데이터**를 가지고 오면 기본적인 포맷의 규칙이 있기 때문에 자동으로 변환돼서 실제 객체 데이터처럼 출력이 되는 것이다.

<br>

- 예제4 (main.js)

  ```jsx
  import myData from "./myData.json";

  console.log(myData);

  const user = {
    name: "userName",
    age: 85,
    emails: ["userEmail@gmail.com", "userEmailTwo@gmail.com"],
  };
  console.log("user", user);

  const str = JSON.stringify(user); // JSON은 전역 객체
  console.log("str", str);
  console.log(typeof str);

  const obj = JSON.parse(str);
  console.log("obj", obj);
  ```

  ![](https://velog.velcdn.com/images/nu11/post/a7082709-11b8-453d-895b-f634bab6c533/image.jpg)

  1. 대문자로 작성된 `JSON` 은 자바스크립트 전체 영역에서 사용할 수 있는 전역 객체다.
  2. `JSON` 은 사용되는 용도가 명확하기 때문에 최대한 경량화를 하는 구조이며, 단순히 하나의 메모리만을 참조하는 큰 덩어리의 문자 데이터로 관리된다.
  3. 자바스크립트 데이터를 `JSON` 로 JSON 포맷화, 즉 문자 데이터로 변환 시키는 것은 `JSON.stringify()` 메소드를 사용하면 된다. 따라서 `str` 을 출력해 보면 객체 형태를 가진 문자 데이터로 출력되는 것이다. `typeof` 를 통해 `str` 의 데이터를 확인해 보면 문자 데이터인 `string` 이 출력되는 것을 확인할 수 있다.
  4. 반대로 `JSON` 파일을 분석하여 재조립, 즉 실제 자바스크립트 데이터로 변환 시키는 것은 `JSON.parese` 메소드를 사용하면 된다. 따라서 `obj` 를 출력해 보면 문자 데이터 `str` 이 다시 객체 데이터로 출력된다.

<br>

## 얕은 복사(Shallow copy), 깊은 복사(Deep copy)

- 예제1

  ```jsx
  const user = {
    name: "Heropy",
    age: 85,
    emails: ["email@gmail.com"],
  };
  const copyUser = user;
  console.log(copyUser === user);

  user.age = 22;
  console.log("user", user);
  console.log("copyUser", copyUser);
  ```

  1. `user` 라는 변수에 객체 데이터가 할당되어 있는데 속성으로는 `name` `age` `emails` 라는 속성을 가지고 있다. 이는 문자, 숫자, 배열 데이터에 해당된다.
  2. `user` 객체 데이터를 `copyUser` 라는 변수에 할당하고 있다.
  3. 일치연산자를 통해 `copyUser` 와 `user` 가 일치하는지 확인하고 있는데 결과값은 `true` 를 출력하고 있다. 이는 같은 메모리 주소를 가리키고 있다는 뜻이다.
  4. 이때 일어날 수 있는 문제를 살펴보면, `user` 의 `age` 속성을 숫자 데이터 `22` 로 바꾸었을때 같은 메모리 주소를 가리키고 있는 `copyUser` 의 `age` 속성 또한 숫자 데이터 `85` 에서 `22` 로 바뀌게 된다.
  5. 참조형 데이터를 할당 연산자로 다른 변수에 할당할 때는 이런 문제를 야기 시킨다. 이때 이러한 문제를 방지하기 위해서 **복사**라는 개념을 사용할 수 있다.

<br>

- 예제2 (얕은 복사, Shallow copy)

  ```jsx
  const user = {
    name: "Heropy",
    age: 85,
    emails: ["email@gmail.com"],
  };
  const copyUser = Object.assign({}, user);
  console.log(copyUser === user);

  user.age = 22;
  console.log("user", user);
  ```

  1. `const copyUser = Object.assign({}, user)` 부분을 살펴보자.
     첫 번째 인수로는 빈 객체 데이터 `{}` 를 넣어주고, 두 번째 인수로는 `user` 를 넣어주자. `{}` 는 대상 객체, `user` 는 출처 객체에 해당된다. 출처 객체 `user` 의 속성들을 복사해서 대상 객체 `{}` 에 담아서 `Object.assign()` 메소드를 실행해서 `copyUser` 에 반환한다.
  2. `{}` 는 리터럴 방식의 객체 데이터고 참조형 데이터이기 때문에 만들어질 때 새로운 메모리 주소를 생성한다. 이는 `copyUser` 가 새로운 메모리 주소에 할당되는 것이므로 `copyUser` 와 `user` 를 일치연산자로 비교해 보면 `false` 를 출력한다. 따라서 `user` 의 `age` 속성을 숫자 데이터 `22` 로 변경해도 `copyUser` 는 별도의 다른 메모리 주소를 가리키고 있으므로 `copyUser` 에는 영향을 미치지 않는다. 이것이 **복사**라는 개념이다.
  3. `Object.assign()` 뿐만 아니라 전개 연산자 `...` 로도 복사를 할 수 있는데 `const copyUser = {...user}` 로 수정해도 복사된 똑같은 결과를 출력한다.

  #### 💡

  `Object.assign()` , `전개 연산자(...)` 로 복사한 개념은 **얕은 복사(Shallow copy)** 에 해당된다.

<br>

- 예제3 (얕은 복사)

  ```jsx
  const user = {
    name: "Heropy",
    age: 85,
    emails: ["email@gmail.com"],
  };

  const copyUser = Object.assign({}, user);
  console.log(copyUser === user);

  user.age = 22;
  console.log("user", user);
  console.log("copyUser", copyUser);

  console.log("-----");
  console.log("-----");

  user.emails.push("pushEmail@gmail.com");
  console.log(user.emails === copyUser.emails);
  // console.log('user', user)
  // console.log('copyUser', copyUser)
  ```

  1. `user` 객체의 `emails` 속성의 배열 데이터에 `push()` 메소드를 사용했다. `push()` 메소드는 배열 데이터의 가장 뒷 부분에 새로운 아이템으로 메소드의 인수 부분을 밀어 넣는다.
  2. 그리고 일치연산자를 사용하여 `user` 의 `emails` 속성과 `copyUser` 의 `emails` 속성을 비교했을때 `true` 라는 값이 출력됐다.
  3. 분명 얕은 복사(Shallow copy)를 사용해서 새로운 `copyUser` 를 만들었는데 어떻게 `user` 와 `copyUser` 의 `emails` 배열 데이터속성을 비교 했을때 `true` 가 나올 수 있을까? 그 이유는 `user` 는 객체 데이터, `emails` 는 배열 데이터로써 둘 다 **참조형 데이터**이기 때문인데 우리는 `user` 객체 데이터만 복사(겉)를 했을 뿐이어서 그 안에 들어 있는 속성 `emails` 의 배열 데이터는 따로 복사 되지 않고 똑같은 메모리 주소를 공유하고 있기 때문이다.
  4. 따라서 `user` , `copyUser` 의 `age` 속성은 서로 다른 숫자 데이터를 가지고 있지만 `emails` 배열 데이터 속성은 `['emails@gmail.com', 'pushEmail@gmail.com']` 두 개의 아이템을 가지고 있다. 이는 얕은 복사를 통해 표면(겉)만 복사 되었기 때문이다.
  5. 이때 필요한 것이 바로 **‘깊은 복사(Deep copy)’** 다. 깊은 복사의 경우에는 순수 자바스크립트로 구현하기가 복잡하다. `lodash` 패키지의 도움을 받아서 구현해 보자.

<br>

### .cloneDeep()

- 예제1 (깊은 복사 Deep copy)

  ```jsx
  import _ from "lodash";

  const user = {
    name: "Heropy",
    age: 85,
    emails: ["email@gmail.com"],
  };
  const copyUser = _.cloneDeep(user);
  console.log(copyUser === user);

  user.age = 22;
  console.log("user", user);
  console.log("copyUser", copyUser);

  console.log("-----");
  console.log("-----");

  user.emails.push("pushEmail@gmail.com");
  console.log(user.emails === copyUser.emails);
  console.log("user", user);
  console.log("copyUser", copyUser);
  ```

  1. 터미널에 `$npm i lodash` 를 입력해서 `lodash` 패키지를 설치한다. 이때 `lodash` 패키지는 웹 브라우저에서 직접 동작을 할 수 있어야 하기 때문에 개발 의존성에 해당하는 `-D` 를 붙이지 않는다.
  2. `main.js` 로 돌아와서 설치된 `lodash` 패키지를 불러오기 위해 최상단에 `import _ from 'lodash'` 를 작성한다. `lodash` 에서 추천하는 방식은 `import lodash` 가 아닌 언더바 기호 `_` 즉, low dash 를 `import` 에 작성하는 것이다. 언더바 기호가 하나의 객체 데이터라고 해석할 수 있다.
  3. `const copyUser =` 에 `lodash` 기능을 사용할 건데 `_` 기호를 시작으로 `_.cloneDeep()` 메소드를 사용하는데 그 인수로 `user` 객체 데이터를 넣어주면 `const copyUser = _.cloneDeep(user)` **깊은 복사**가 완성된다. `clone` 은 복제, `Deep` 은 깊은 이라는 뜻으로 깊은 복제가 된다.
  4. 따라서 일치연산자를 통해 `copyUser` 와 `user` 를 비교했을때 `false` 값을 확인할 수 있으며 `user.age = 22` 로 했을때 `user` 는 `22`, `copyUser` 는 `85` 로 되어 있으며 이전과는 다르게 `user.emails.push` 를 했을때 `user.emails === copyUser.emails` 는 `false` 값을 나타낸다. 그 증거로 `user` 와`copyUser` 를 출력해보면 `emails` 의 배열 데이터 아이템 개수는 `push` 된 것을 포함해 2개 이고, `copyUser` 의 `emails` 의 아이템 개수는 1개이다.

  #### 💡

  참조형 데이터를 복사할 때 얕은 복사로도 충분히 문제없이 복사가 된다면 `Object.assing()` , `전개연산자(...)` 를 사용해서 참조형 데이터를 복사해도 된다. 하지만 참조형 데이터가 내부에 또 다른 참조형 데이터를 포함하고 있다면 깊은 복사를 해주는 것이 더 안전한다. 이때 깊은 복사는 순수 자바스크립트로는 구현이 복잡하니 `lodash` 패키지 도움을 받아서 `_.cloneDeep()` 이라는 패키지 내부 메소드를 통해 손쉽게 깊은 복사를 하자.

### \_.cloneDeep(value)

This method is like `_.clone` except that it recursively clones value. `cloneDeep` 메소드는 `Deep` 이 붙어있지 않은 `_.clone` 메소드와 유사한데 정확하게는 재귀적인 적인 값을 복사한다.

> 재귀란 하나의 데이터 안에서 어떠한 내용이 반복적으로 실행되는 것을 의미한다. 쉽게 말해 **반복실행**이다.

즉, `_.cloneDeep()` 메소드는 반복실행 하면서 모든 값들을 복제하면서 깊은 복사가 가능해진다는 것이다.

- 예제1

  ```jsx
  var objects = [{ a: 1 }, { b: 2 }];

  var deep = _.cloneDeep(objects);
  console.log(deep[0] === objects[0]);
  // expected output: false
  ```

  `objects` 의 참조형 데이터인 `[]` 배열 데이터만 복사한 것이 아닌 그 안에 들어있는 또 다른 참조형 데이터인 객체 데이터 `{}` 까지 모두 복사했다는 것을 알 수 있다.

<br>

## API 호출

- API 호출을 레스토랑으로 비교

  ![](https://velog.velcdn.com/images/nu11/post/7be0d14b-46ca-4d86-9738-54ccae2b9b03/image.PNG)

  ![](https://velog.velcdn.com/images/nu11/post/f9cc768a-a101-4eca-96f2-76821ceb5d26/image.PNG)

<br>

## ES2022

> 노마드코더 - 방금 출시된 ⚡️자바스크립트 미친 신기능 4개!
https://youtu.be/m-R7s7fnwvU

위 영상을 보며 메모한 것을 기록으로 남겨 둡니다.

<br>

### Top Level Await

`await startServer();` 
await을 async 함수 없이 써도 된다!

<br>

### Error cause

```js
// 이전
new Error("오류 메시지")

// ES2022!!!
new Error("오류 메시지", { cause: "무엇이 잘못되었는지 구체적으로 작성1" });
new Error("오류 메시지", { cause: "무엇이 잘못되었는지 구체적으로 작성2" });
```
덕분에 동일한 오류 메시지를 사용하지만 다른 원인을 작성할 수도 있음!
`.cause`로 액세스도 가능!

<br>

### .at()
```js
const emoji = ["👍", "🎶", "🐙"];
emoji.at(0); // 👍
emoji.at(-1); // 🐙
```
배열의 모든 인덱스에 액세스 할 수 있음!  `emoji[2]` 로도 접근 가능했으나, `at(-1)` 을 사용하면 뒤로 검색할 수 있음!

<br>

### Class Fields
자바스크립트가 OOP를 위한 성숙한 언어처럼 느껴지게 만듬! 불가능 했던 Private 메서드 및 속성을 가질 수 있고 static 메서드도 만들 수 있음!

<br>

### ES2022 메모
`Top Level Await` 이랑 `.at()` 엄청 유용해 보인다!
특히 배열에서 뒤로 검색할 때는 `emoji[emoji.length -1]` 처럼 사용했었는데 이제 쉽게 접근 가능해서 좋아 보인다!

<br>