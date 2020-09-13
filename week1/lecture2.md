# 2020.09.03 2일차

<details> <summary> 📌 Table of Contents </summary>

  - [🧐 강의 목표](#강의-목표)
  - [💡 Javascript A to Z](#javascript-a-to-z)
    - [변수](#변수)
    - [함수](#함수)
    - [함수 정의문](#함수-정의문)
    - [함수 정의식](#함수-정의식)
    - [IIFE (즉시 호출 함수)](#iife-즉시-호출-함수)
    - [callback 함수](#callback-함수)
    - [재귀호출 함수](#재귀호출-함수)
    - [식과 문](#식과-문)
    - [new 연산자](#new-연산자)
    - [this와 실행 컨텍스트](#this와-실행-컨텍스트)
    - [클로저 (closure)](#클로저-closure)
  - [💡 비동기와 Promise](#비동기와-promise)
    - [비동기 setTimeout 함수](#비동기-settimeout-함수)
    - [Promise](#promise)
    - [Async와 Await](#async와-await)
  - [💡 Redux 만들어보기](#redux-만들어보기)

</details>
## 🧐 강의 목표

- Javascript A to Z
- React and Mobx

---

## 💡 Javascript A to Z

### 변수

- 값으로 취급되는 모든 것은 변수에 넣을 수 있음
- 함수, 객체, 숫자 등..

### 함수

- 모든 함수는 값을 반환함
- 함수는 값으로 취급되어 변수에 대입이 가능함
- 함수를 정의하는 방법은 `함수 정의문` 과 `함수 정의식`

### 함수 정의문

```tsx
function () {}
```

### 함수 정의식

```tsx
const bar = function bar () {}; 
bar(); //호출한 bar()는 const로 선언한 bar이다. 

//함수를 값으로 취급 할 땐 이름을 생략 할 수 있다.
const bar2 = function () {}; 

```

### IIFE (즉시 호출 함수)

- 오직 단 한번만 실행되는 함수
- 익명함수를 `()`로 감싸 `즉시 호출` 함

```tsx
(function () {...})(); 
```

### callback 함수

- 함수를 리턴하는 함수
- High Order Function (HOC)
    - 함수를 인자로 받고 함수를 리턴 하는 함수
    - 1급 함수라고도 함

```tsx
function foo(x) {
    x(); 
    return function() {}; 
} 

const y = foo(function () {}); 
```

### 재귀호출 함수

- `재귀호출` 할 땐 오른쪽에 대입된 함수의 이름을 사용

```tsx
const foo = function foo () {
    foo() 
    // function foo의 foo가 호출된다. 
    // 재귀호출 할 땐 익명함수이면 안되고 foo라는 이름이 있어야됨
}
```

### 식과 문

- 실행 결과가 `값`이면 `식`
    - 0, 1+10, foo() 호출 결과 값
    - `세미콜론(;)`으로 식을 마무리

- 실행 결과가 값이 아닌 경우 `문`
    - `if` , `while`
    - `세미콜론(;)`이 없음

### new 연산자

```tsx
function () {
	{} //빈 객체 생성 
	this // new 연산자 호출 했을때 js엔진이 생성하는 빈 객체 
	this.fds = 10; //동적 바인딩
} 

const y = new foo(); 
//새로만든 this가 리턴되어 y에 들어감 

if(y instanceof foo) {
//y가 foo의 인스턴스 인지 검사 
}
```

### this와 실행 컨텍스트

- getName() 실행
- 자바스크립트 엔진이 getName()을 호출하는 소유자를 확인함
- 소유자가 person임을 확인

```jsx
const person = {
	name : 'kim', 
	getName () {
		return this.name; 
	}
}

console.log(person.getName()) // kim 
//getName 호출 시 실행하는 소유자가 누군지 확인함 
// this는 person이 된다. 

const man = person.getName; //소유자가 벗겨짐 
//this 는 global이 된다. 
console.log(man()); //Error ! 

**
person.getName.bind(person) 
//소유자가 벗겨져도 this가 무조건 person을 가리킴 
```

- `this`가 결정되는 방식은 호출 시점에 결정 됨 (`실행 컨텍스트`)
- `this`를 고정 하기 위해서 `bind` , `call`, `apply` 사용

### 클로저 (closure)

```jsx
const person = {
	age : 10
} 

function makePerson() {
	let age = 10; 
	return {
	    getAge() {
		    return age; 
	    }

	    setAge(x) {
	    	age = x > 1 && x < 130 ? x : age;
	    }
        }
}

let p = makePerson(); 
```

## 💡 비동기와 Promise

### 비동기 setTimeout 함수

```jsx
setTimeout( function foo () {
	console.log('hello1'); 

	setTimeout( function(y) {
		console.log('hello2'); 
	},2000); 

}, 1000);

//hello1 (1초뒤) 
//hello2 (2초뒤)
```

아래 코드 처럼 비동기 로직이 `n depth` 로 계속 중첩 되는 것을 `콜백 지옥` 이라고 부른다. 

```jsx
setTimeout(function () {
	console.log('foo1'); 
	setTimeout(function () {
		console.log('foo2');
		setTimeout(function () {
			console.log('foo3'); 
			//...
		});
	});
});
```

### Promise

`콜백 지옥`을 해결하기 위해 `Promise`가 등장했다. 두둥 

```jsx
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve('resolved'); 
	}, 1000); 
}); 

const p2 = new Promise((resolve, reject) => {
	seTimeout(() => {
		resolve('resolved2'); 
	}, 1000); 
}); 

p1.then(function (r) {
	return p2; 
}).then(function (r) {
	console.log(r); 
}).catch(function () {
	//...에러 발생...
}); 
```

- Promise가 `성공` 했을 때 콜백함수로 `resolve()`가 호출
- Promise가 `실패` 했을 때 콜백함수로 `reject()`가 호출
- `then()`을 사용하여 이전에 리턴된 값을 받아 다음 프로미스로 넘겨준다.

### Async와 Await

- `async`와 `await` 키워드를 이용하면 `Promise`를 가독성 좋게 작성할 수 있음
- `await` 키워드는 `async` 가 붙은 함수 안에서만 사용이 가능함
- Promise의 `reject()`는 `catch`를 이용해서 구현할 수 있음

```jsx
const delay = ms => new Promise(resolve => setTimeout(ms)); 

async function main() {
	console.log('1'); 
	try {
		const res = await delay(3000); 
	}catch (err) {
		console.error(err); 
	} 
	console.log('2'); 
} 

main(); 

// 1 
// 3초 대기 
// 2 
```

## 💡 Redux 만들어보기

- 하나의 앱에 `하나의 스토어`만 존재 해야 함
- 컴포넌트는 redux의 상태를 직접적으로 바꿀 수 없음
    - 여러 컴포넌트가 store의 데이터를 쓸 수 있는데, 한곳에서 바꿔버리면 `변경된 state`를 통보 받는데에 문제가 있다.

`redux.js`

```jsx
export function createStore(reducer) {
  let state;
  const listeners = [];

  const getState = () => ({ ...state });

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach((fn) => fn());
  };

  const subscribe = (fn) => {
    listeners.push(fn);
  };

  return {
    getState,
    dispatch,
    subscribe
  };
}
```

`index.js`

```jsx
import { createStore, dispatch } from "./redux";

const INCREMENT = "increment";
const RESET = "reset";

function reducer(state = {}, action) {
  if (action.type === INCREMENT) {
    //기존 state의 의존성을 끊고 새로운 state를 반환.
    return { ...state, count: state.count ? state.count + 1 : 1 };
  } else if (action.type === RESET) {
    return {
      ...state,
      count: 0
    };
  }
  return state;
}

const store = createStore(reducer);

//reducer의 store가 바꼈을 때 호출 됨
function update() {
  console.log(store.getState());
}

store.subscribe(update);

function actionCreator(type, data) {
  return {
    ...data, //data는 오브젝트가 올수있어서 spread함. 앞에서 spread해야 뒤에 type이 안묻힘
    type: type
  };
}

function increment() {
  store.dispatch(actionCreator(INCREMENT));
}

function reset() {
  store.dispatch(actionCreator(RESET));
}

increment();
increment();
increment();
increment();
reset();
increment();

console.log(store.getState());
```
