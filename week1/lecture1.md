# 2020.09.01 1일차

<details><summary> 📌 Table of Contents</summary>

- 💡 [주니어들의 공통된 고민](#주니어들의-공통된-고민)
- 💡 [사용할 툴](#사용할-툴)
- 💡 [Typescript](#typescript)
    - [타입 명시와 추론](#타입-명시와-추론)
    - [Type Alias](#type-alias)
    - [Object 와 Interface](#object-와-interface)
- 💡 [React](#react)
    - [CRA로 React Typescript 템플릿 설치](#cra로-react-typescript-템플릿-설치)
    - [Babel을 이용해 React 컴포넌트 변환](#babel을-이용해-react-컴포넌트-변환)
    - [CRA를 현업에서 사용하면 힘든점](#cra를-현업에서-사용하면-힘든점)
</details>

<br>

## 💡 주니어들의 공통된 고민

- 내가 옳은 길을 가고있나..?
- 아키텍처에 대한 고민
- 사수가 없어요


## 💡 사용할 툴

[Playground - An online editor for exploring TypeScript and JavaScript](https://www.typescriptlang.org/play/)

[CodeSandbox](https://codesandbox.io/index2)

[React](https://reactjs.org/)

[Redux](https://redux.js.org/)

[MobX](https://mobx.js.org/README.html)

[Redux Saga](https://redux-saga.js.org/)

[Blueprint - A React-based UI toolkit for the web](https://blueprintjs.com/)

[Testing Library · Simple and complete testing utilities that encourage good testing practices](https://testing-library.com/)

---

## 💡 TypeScript

### 타입 명시와 추론

```tsx
let foo : number = 10; //타입 명시 

let foo = 10; //타입 추론   
// foo 라는 변수에 명시적으로 10 이라는 값을 넣어줘서 number 타입이라는 걸 추론함. 
```

- 타입 명시가 직관적이고 읽기 쉬움

### Type Alias

```tsx
type Age = number; 
let age : Age = 10; 
```

- 타입에 대한 이름을 붙여줌

### Object 와 Interface

```tsx
type Age = number; 
type Foo = {
	age : Age; 
	name : string; 
} 

interface Bar {
	age : Age;
	name : string; 
} 

const foo : Foo = {
	age : 10, 
	name : 'kim' 
} 

const bar : Bar = {
	age : 10, 
	name : 'lee' 
} 
```

---


## 💡 React

### CRA로 React Typescript 템플릿 설치

```tsx
yarn create react-app tech-hello --template typescript
```

### Babel을 이용해 React 컴포넌트 변환

- index.tsx

```tsx
import React from 'react';

function App () {
	return (
		<h1 id="header"> Hello? </h1>
	)
}
```

- 트랜스 파일링 된 결과물

```tsx
"use strict";

var _react = _interopRequireDefault(require("react"));

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function App() {
  return /*#__PURE__*/_react.default.createElement("h1", {
    id: "header"
  }, "Hello");
}
```

> react.default.createElement 함수를 사용하기 때문에 React를 import 하지 않으면 에러가 발생함


---


### CRA를 현업에서 사용하면 힘든점

- 여러가지 설정이 프리셋으로 구성되어있는 점은 좋지만 그만큼 커스터마이징 하기 어려울 수 있음
- `eject`를 이용하면 커스터마이징 할 수 있지만  CRA의 설정을 모두 알아야 하기 때문에 복잡
- `craco` , `react-app-rewired` 등 CRA 커스텀으로 오버라이드 라이브러리도 존재함