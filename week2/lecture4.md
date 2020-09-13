# 2020.09.10 4일차

<details><summary> 📌 Table of Contents</summary>

  - [🧐 강의 목표](#강의-목표)
  - [💡 컴포넌트 아키텍처 잡기](#컴포넌트-아키텍처-잡기)
    - [sessionList 컴포넌트화](#sessionlist-컴포넌트화)
  - [💡 함수형, 클래스형 컴포넌트에서의 State](#함수형-클래스형-컴포넌트에서의-state)
    - [클래스형 컴포넌트](#클래스형-컴포넌트)
    - [함수형 컴포넌트](#함수형-컴포넌트)
  - [💡 Generator 와 Async 함수](#generator-와-async-함수)
    - [Generator (제너레이터)](#generator-제너레이터)
    - [Async (비동기) 함수](#async-비동기-함수)
  - [❓ QnA](#qna)
  
</details>

## 🧐 강의 목표

- 상태 디자인 하는 방법
- Redux

---

## 💡 컴포넌트 아키텍처 잡기

`index.js`

```tsx
import React from "react";
import ReactDOM from "react-dom";

import App from "./App";

const rootElement = document.getElementById("root");

const sessionList = [
  { title: "1회차: Overview" },
  { title: "2회차: Redux 만들기" },
  { title: "3회차: React 만들기" },
  { title: "4회차: 컴포넌트 디자인 및 비동기" }
];

ReactDOM.render(
  <React.StrictMode>
    <App store={{ sessionList }} />
  </React.StrictMode>,
  rootElement
);
```

`App.js`

```tsx
import React from "react";

const App = (props) => {
  const { sessionList } = props.store;

  return (
    <div>
      <header>
        <h1> React and TypeScript </h1>
      </header>
      <ul>
        {/* jsx안에 로직 코드를 적게되면 가독성이 떨어짐..  */}
        {sessionList.map((session) => (
          <li> {session.title}</li>
        ))}
      </ul>
      ;
    </div>
  );
};

export default App;
```

### sessionList 컴포넌트화

- render()의 `JSX` 안에 로직 코드를 작성하게 되면 가독성이 떨어진다.
- sessionList를 title을 props로 받는 컴포넌트로 분리한다.

`컴포넌트화 전` 

```tsx
{sessionList.map((session) => (
   <li> {session.title}</li>
))}
```

`컴포넌트화 후` 

```tsx
const SessionItem = ({title}) => <li> {title} </li>; 

{sessionList.map((session) => (
   <SessionItem title={session.title} />
 ))}
```

---

## 💡 함수형, 클래스형 컴포넌트에서의 State

### 클래스형 컴포넌트

`App.js`

```jsx
export default class App extends React.Component {
  constructor(props) {
    super(props);
		
	//this 바인딩 
    this.onToggleDisplayOrder = this.onToggleDisplayOrder.bind(this);

    this.state = {
      displayOrder: 'ASC',
    }
  }
	
  onToggleDisplayOrder() {
    this.setState({
      displayOrder: displayOrder === 'ASC' ? 'DESC' : 'ASC',
    });
  }
	
  //arrow function 
  toggleDisplayOrder = () => {
    this.setState({
      displayOrder: displayOrder === 'ASC' ? 'DESC' : 'ASC',
    })
  }

  render() {
    return (
      <div>
        <button onClick={this.toggleDisplayOrder}>재정렬</button>
      </div>
    )
  }
}
```

일반 function vs. arrow function

```tsx
 onToggleDisplayOrder() {
     //일반함수는 this가 실행 컨텍스트
     this.setState({
      displayOrder : displayOrder === 'ASC' ? 'DESC' : 'ASC'; 
     })
   }
  
  toggleDisplayOrder = () => {
    //여기서 this는 lexical 
    this.setState({
      displayOrder : displayOrder === 'ASC' ? 'DESC' : 'ASC'; 
    })
  };
```

### 함수형 컴포넌트

- 기존에는 `state`를 사용하려면 클래스형 컴포넌트를 사용해야 했지만, `useState` hooks가 등장 하면서 함수형 컴포넌트 내에서도 `state` 사용이 가능해졌다.

`App.js`

```tsx
import React from "react";

const SessionItem = ({ title }) => <li>{title}</li>;

const App = (props) => {
  const [displayOrder, toggleDisplayOrder] = React.useState("ASC");
  const { sessionList } = props.store;
  const orderedSessionList = sessionList.map((session, i) => ({
    ...session,
    order: i
  }));

  const onToggleDisplayOrder = () => {
    toggleDisplayOrder(displayOrder === "ASC" ? "DESC" : "ASC");
  };

  return (
    <div>
      <header>
        <h1>React and TypeScript</h1>
      </header>
      <p>전체 세션 갯수: 4개 {displayOrder}</p>
      <button onClick={onToggleDisplayOrder}>재정렬</button>
      <ul>
        {orderedSessionList.map((session) => (
          <SessionItem title={session.title} />
        ))}
      </ul>
    </div>
  );
};

export default App;
```

---

## 💡 Generator 와 Async 함수

### Generator (제너레이터)

- 함수를 `function*` 로 선언함

```tsx
function* foo() {}
```

- 일반 함수와의 차이점 : 함수 내부에서 상태를 계속 유지하고 있다.

```tsx
function* makeNumber() {
  let num = 1;

  while (true) {
    //yield 는 generator 안의 리턴
    const x = yield num++;
    console.log(x);
  }
}
const i = makeNumber();

i.next(); //yield나 return 을 만날때 까지 계속 실행
```

### Async (비동기) 함수

- 함수명 앞에 `async` 키워드를 붙임

```tsx
async foo() {} 
```

```tsx
const delay = (ms) => new Promise((resolve) => setTimeout(resolve));
function* main() {
  console.log("start");
  yield delay(3000);
  console.log("3s later..");
}

async function main2() {
  console.log("start");
  await delay(3000); //await는 뒤에 promise가 와야 쓸수있음
  console.log("3s later..");
}
main2();

const it = main();
const { value } = it.next();
value.then(() => {
  it.next();
});
```

---

## ❓ QnA

```tsx
Q. 노션에 학습내용을 정리하고 있는데 오픈된 기술 블로그에 공개하는게 면접관님께 가산이 될까요? 
```

- 가산보다 지표자료가 된다. 어떤 내용. 어떤 기술 수준인지. 참고용으로 쓰임
