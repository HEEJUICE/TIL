# REDUX

---

## 1. ACTION

액션 개체는 type 필드를 반드시 가지고 있어야 하며, 이 값을 액션의 이름이라고 생각한다.

```react
{
  type: 'ADD_TODO',
    data: {
      id: 1,
        text: '리덕스 배우기'
    }
}

{
  type: 'CHANGE_INPUT',
    text: '안녕하세요'
}
```

### 액션 생성 함수

```react
function addTodo(data) {
  return {
    type: 'ADD_TODO',
    data
  }
}

const changeInput = text => ({
  type: 'CHANGE_INPUT',
    text
})
```

어떤 변화를 일으켜야 할 때마다 액션 객체를 만들어야 하는데 매번 액션 객체를 직접 작성하기 번거롭고 만드는 과정에서 실수로 정보를 놓칠 수도 있다. 이러한 일을 방지하기 위해 함수로 만들어서 관리한다.

##### 리듀서(reducer)

```react
const initialState = {
  counter: 1
};
function reducer(state = initialState, action) {
  switch (action.type) {
    case INCREMENT:
      return {
        counter: state.counter + 1
      };
    default:
      return state;
  }
}
```

##### 스토어

프로젝트에 리덕스를 적용하기 위해 스토어를 만듦. 한 개의 프로젝트는 단 하나의 스토어만 가질 수 있다.

- 디스패치

스토어의 내장 함수 중 하나. '액션을 발생시키는 것'. 이 함수는 _dispatch(action)_ 과 같은 형태로 액션 객체를 파라미터로 넣어서 호출. 스토어는 리듀서 함수를 실행시켜서 새로운 상태로 만들어준다.

- 구독

스토어의 내장 함수 중 하나. _subscribe_ 함수 안에 리스너 함수를 파라미터로 넣어서 호출해 주면, 이 리스너 함수가 액션이 디스패치되어 상태가 업데이트될 때마다 호출됨.

```react
const linstener = () => {
  console.log('상태가 업데이트됨');
}
const unsubscribe = store.subscribe(listenenr);
unsubscribe(); //추후 구독을 비활성화할 때 함수를 호출
```

## 2. 리액트 없이 쓰는 리덕스

```react
const TOGGLE_SWITCH = 'TOGGLE_SWITCH';
const INCREASE = 'INCREASE';
const DECREASE = 'DECREASE';

const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increase = difference => ({ type: INCREASE, difference });
const decrease = () => ({ type: DECREASE })

const initialState = {
  toggle: false,
  counter: 0
}

//state가 undefined일 때는 initalState를 기본값으로 사용
function reducer(state = initalState, action) {
  // action.type에 따라 다른 작업을 처리함
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state, //불변성 유지
        toggle: !state.toggle
      };
    case INCREASE:
      return {
        ...state,
        counter: state.counter + action.difference
      };
    case DECREASE:
      return {
        ...state,
        counter: state.counter - 1
      };
    default:
      return state;
  }
}
```

리듀서 함수가 맨 처음 호출될 때는 state값이 undefined이다. 해당 값이 undefined로 주어졌을 때, initialState를 기본값으로 설정하기 위해 함수의 파라미터 쪽에 기본값이 설정되어 있다.

### 스토어 만들기

_createStore_ 함수 사용

```react
import { createStore } from 'redux';

const store = createStore(reducer);
```

### render 함수 만들기

```react
const store = createStore(reducer);

const render = () => {
  const state = store.getState(); //현재 상태를 불러옴.
  //토글 처리
  if (state.toggle) {
    divToggle.classList.add('active');
  } else {
    divToggle.classList.remove('active')
  }
  //카운터 처리
  counter.innerText = state.counter;
}
```

### 구독하기

스토어의 상태가 바뀔 때마다 render 함수가 호출되도록 함. _subscribe_ 함수 사용.

**_react 에서는 subscribe 함수를 직접 사용하지 않음. 컴포넌트에서 리덕스 상태를 조회하는 과정에서 'react-redux'라는 라이브러리가 이 작업을 대신해줌_**
