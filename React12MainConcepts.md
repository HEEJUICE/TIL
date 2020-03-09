# React 12 Main Concepts 1편

React 12 main concepts의 공식 문서를 번역함과 동시에 요약한 글이다. 모든 자료의 출처는 React공식 홈페이지에 있는 자료들이다.

우선, 12가지 main concepts의 구성은,

1. Hello World
2. Introducing JSX (JSX 소개)
3. Rendering Elements (요소 렌더링)
4. Components and Props (Components와 Props)
5. State and Lifecycle (State와 Lifecycle)
6. Handling Events (이벤트 제어하기)
7. Conditional Rendering (조건부 렌더링)
8. Lists and Keys (Lists와 Keys)
9. Forms (폼)
10. Lifting State Up(State 끌어올리기)
11. Composition vs Inheritance (구성 vs 상속)
12. Thinking in React (React스럽게 생각하기)
    이다.

이번 1편에서는 5번까지 다룬다.

---

## 1. Hello World

Hello, World 파트는 말 그대로 React를 소개하는 파트이다.

```js
ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById("root"));
```

이것이 React를 사용하여 Web에 'Hello, world!'를 띄우는 기본이다.

## 2. Introducing JSX

JSX의 문법은 흡사 HTML 문법과 비슷하게 생겼지만 그렇지 않다. React에서 쓰이는 JS의 확장문법이다.

```js
const element = <h1>Hello, world!</h1>;
```

라는 식으로 쓸 수 있다.

JSX가 React를 사용함에 있어서 필수는 아니지만, 대부분의 사람들이 JS안에서 UI작업을 할 때 JSX를 사용하는 것이 더 낫기 때문에 사용한다. 또한, 보다 도움이 되는 에러나 경고 메세지를 줄 수 있기 때문이다.

```js
function formatName(user) {
  return user.firstName + " " + user.lastName;
}

const user = {
  firstName: "Harper",
  lastName: "Perez"
};

const element = <h1>Hello, {formatName(user)}!</h1>;

ReactDOM.render(element, document.getElementById("root"));
```

JSX에서는 위에서 볼 수 있듯이 JS 코드를 적용할 때는 중괄호 안에 작성한다.
그리고, 지금은 < h1>태그 하나이지만 여러개의 elements를 작성할 때는 무조건 하나의 elements로 감싸야 한다.
전체를 감싸주는 < div>를 만들어주고(HTML에서 < body>안에 적는 것 처럼) 그 안에서 여러개의 elements를 작성할 수 있다.

JSX 내부에서는 if문 사용이 안되기 때문에 IIFE나 삼항연산자를 사용해야 하지만 JS내에서는 if문이나 for문에서 JSX사용할 수 있다. 아래 예시처럼.

```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

JSX에서 태그 안에서 어떤 것을 정의해줄 때는 따옴표를 사용할 수 있다. HTML 문법하고 똑같다. tabIndex라는 속성에 정의를 주고 싶을 땐 "0"이라고 따옴표를 사용해서 정의해주면 된다.

```js
const element = <div tabIndex="0"></div>;
```

JS코드는 속성 안에 적을 때도 중괄호를 사용한다.

```js
const element = <img src={user.avatarUrl}></img>;
```

같은 속성에 정의 내릴 때, 중괄호와 따옴표를 동시에 사용할 수 없다.

> **Warning:**
> JSX는 보기에는 HTML하고 비슷한 점이 많아도 코드 구동 방식은 JS에 더 가깝기 때문에 **"camelCase"** 라는 속성 이름 컨벤션(모음집 같은...?)을 사용한다.
> 예를 들어서, class 는 **ClassName**이 되고, tabindex는 **tabIndex**가 된다.

### JSX의 자식 정의

만약에 태그가 비어있다면 /> 를 이용해서(XML처럼) 닫아주어야 한다.

```js
const element = <img src={user.avatarUrl} />;
```

JSX태그는 자식 태그도 가질 수 있다.

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX를 사용해서 XSS공격을 막는 방법

유저 입력은 JSX내에 입력하는 것이 안전하다.
왜냐하면 React DOM은 렌더링 하기 전에 JSX내에 있는 값들을 미리 빼놓는다. 그렇기 때문에 절대로 JSX내에 미리 입력되지 않은 값들을 임의로 삽입할 수 없게 되어있다. 렌더링 하기 전에 모든 것은 string으로 변환되기 때문에 XSS 공격을 막을 수 있게 된다.

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

### JSX 객체 표현

> ES6 및 JSX 코드가 모두 표시되도록 "Babel"이라는 언어를 사용하기를 권장한다.

```js
const element = <h1 className="greeting">Hello, world!</h1>;
```

과

```js
const element = React.createElement(
  "h1",
  { className: "greeting" },
  "Hello, world!"
);
```

은 동일한 코드이다. Babel은 JSX를 React.createElement() 호출로 컴파일 한다.
그러므로 "React elements"라고 불리우는 아래와 같은 객체를 생성해낸다. 이것을 사용하여 DOM을 구성한다.

```js
// Note: this structure is simplified
const element = {
  type: "h1",
  props: {
    className: "greeting",
    children: "Hello, world"
  }
};
```

## 3. Rendering Elements

Elements는 React app에서 가장 작은 구성 블럭이다. Elements는 내가 스크린에 보이고 싶은 것을 말한다. Browser DOM과 달리, React elements는 순수한 오브젝트이고 생성하기가 더 훨씬 간편하고 수월하다. React DOM은 React elements와 Browser DOM이 일치되도록 업데이트한다. (\*Component와 헷갈림 주의!)

### DOM에 Element 렌더링 하기

우선, HTML에 div 하나를 만들자.(DOM node) 이 div의 class나 id명은 "root"라고 지을 수 있다. HTML안에 이 root div하나만 있으면 모든 것을 React DOM에서 관리할 수 있기 때문이다. 보통 이 root DOM node는 React app당 하나만 만들어도 되지만 여러개의 React app을 합칠 때에는 그만큼 여러개의 root DOM이 존재할 수 있다.

React element를 DOM에 렌더링 할 때는 **ReactDOM.render()** 을 사용한다.

```js
const element = <h1>Hello, world</h1>;

ReactDOM.render(element, document.getElementById("root"));
```

### 이미 렌더링된 Element 업데이트 & React는 필요한 부분만 업데이트 한다.

React elements는 변경할 수 없기 때문에 한 번 element를 생성하면 그 속성이나 자식요소 또한 변경할 수 없다. 무조건 새로운 element를 만들고 다시 렌더링 해야한다.

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById("root"));
}

setInterval(tick, 1000);
```

이런 식으로 시간이 깜박이는 예제를 볼 때 setInterval() 콜백을 이용해 매 초마다 ReactDOM.render()를 호출하고 있지만, 실제로 대부분의 React 어플리케이션은 ReactDOM.render()를 한 번만 호출한다. 이러한 문제(?)는 다음 Component 파트에서 배울 수 있다.

그리고, 저 예제에서 우리가 확인할 수 있는 또 다른 사실은 new Date()는 h2태그 안에 다른 string이랑 같이 있다. 하지만 시간이 바뀔 때 DOM이 어떻게 바뀌는 지 보면 'It is'와 같은 string 부분은 바뀌지 않고 오로지 시간을 나타내는 부분만 새로 업데이트 됨을 알 수 있다.

_\*시간이 지남에 따라 UI를 변경하는 것이 아니라 주어진 순간을 UI가 어떻게 보여줘야 하는 지 고민하자!_

## 4. Components and Props

컴포넌트는 UI를 분리해서 독립적이고 재사용 가능하다고 볼 수 있다. 컴포넌트는 JS함수와 비슷하다. 하지만 State라는 것을 사용하기 위해서는 ES6 class를 사용하는 것이 좋다. functional은 State를 사용할 수 없다.

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

이 예제는 일단 functional로 component를 구현한 모습이다. 이 코드를 실행시키면 "Hello, Sara" 라는 문구가 뜰 것이다.
실행 순서는,

1. ReactDOM.render()로 < Welcome name ="sara"/> element를 가져온다.
2. React는 Welcome component를 호출하는데, 여기서 {name: 'Sara'}라는 props를 가져온다.(props는 object형태이다.)
3. Welcome component는 < h1>Hello, Sara< /h1> element를 결과로서 반환한다.
4. React DOM이 Hello, Sara 와 일치하도록 DOM을 업데이트한다.

_**요약 : 함수처럼 생긴 component를 생성해주고 element안에서 component의 값을 정의해준다. 이 값은 props라고 한다.**_

> Component 이름은 항상 대문자로 시작해야 한다.
> 예를 들어서 <div/ >는 DOM 태그를 나타내지만 <Welcome/ >은 component를 나타낸다. 그리고 스코프 내에서 Welcome component를 요구한다.

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

이렇게 App이라는 component를 새로 생성해주고 그 안에다 Welcome component를 여러개 실행시키면 output은 Hello, 'name'이 3개가 출력된다.
component는 재사용 가능하게끔 최대한 따로 추출해주는 것이 좋다.
Props는 읽기 전용인데, 예를 들어서 '순수 함수' 라고 불리우는 것이 있다.

```js
function sum(a, b) {
  return a + b;
}
function withdraw(account, amount) {
  account.total -= amount;
}
```

이 두 개의 함수를 비교했을 때, return 했을 때 parameter로 들어온 값이 위의 함수는 변하지 않는다. 하지만 아래 함수는 return 하면서 amount의 값이 변하기 때문에 '순수 함수'가 아니다.
React에서는 엄격하게 지켜야 하는 규칙이 있는데 이것은,

**"모든 React components는 props를 순수 함수처럼 동작하게끔 만들어야 한다."**

하지만, state를 활용하면 이 규칙을 어기지 않고도 출력되는 값이 변하게 만들 수 있다.

## 5. State and Lifecycle

이 파트는 React component내에서 사용되는 개념인 State와 Lifecycle을 소개한다.
State는 props와 비슷하지만 props와는 개별적인 개념이고 component에 의해 완전히 컨트롤되는 아이이다.
이제 State를 사용하기 위해서 위에서 작성했던 functional component는 쓰지 않고 class로 component를 작성하는 법을 동시에 배울 것이다.

```js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(<Clock date={new Date()} />, document.getElementById("root"));
}

setInterval(tick, 1000);
```

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

1. ES6 class를 같은 이름으로 만들고 React.Component를 확장시킨다.
2. render()라고 불리우는 빈 method를 입력한다.
3. functional에 있던 body부분을 render() method안에 넣는다.
4. 원래 props였던 부분은 this.props 로 변경한다.
5. 이제 비어버린 남아있는 tick()함수는 제거한다.

이렇게 functional component를 class로 바꿀 수 있다. (위 functional component에 쓰여진 'Clock date = {new Date()}부분은 잘못된 방법이기 때문에 class로 바꾼 후, state를 사용하여 알맞게 고칠 것이다.)
