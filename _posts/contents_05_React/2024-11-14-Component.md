---
layout: post
title: "React_01_Component"
date: 2024-11-14
categories: blog
category: React
---

<br>

---

# ⚡️ React

<h3>&emsp;리액트는 사용자 정의 태그를 만드는 기술</h3>
> 출처 : 생활코딩 React 리액트 프로그래밍, 이고잉 저


<details>
<summary>터미널로 실행하기</summary>
<div markdown="1">
Node.js -> 플랫폼을 위해 설치 <br>
설치 주소 : [https://nodejs.org/en](https://nodejs.org/en)
</div>

프로젝트 생성 및 실행 
<pre><code>
// 프로젝트 폴더 생성
%> npx create-react-app 프로젝트명

// 실행 (실습용 서버로 실행 _ 포트번호 3000)
%> npm start 
// 위와 동일
%> npm run start 
</code></pre>

</details>

<br>
<hr>
<br>

## ☑️ Virtual DOM
<pre>
  DOM (Document Object Model)
    html, xml, CSS와 같은 문서를 위계가 있는 트리구조로 인식하여
    각 테이터(요소 : Element)를 객체로 취급, 관리하는 방식

  React는 컴포넌트의 상태에 변화가 생기면 
  가상의 DOM을 생성하여 변경 사항을 비교한다. "diffing"
  실제 DOM의 해당 부분만 업데이트하기 때문에 성능이 향상된다.

  이 때 순수 JS에서 DOM에 접근하기 위해 사용하던 
  <i>document.getElementById()</i>나 <i>document.querySelector()</i>와 같은
  접근 방식은 권장되지 않는다.

  실제 DOM에 접근하기 때문에 상태나 속성 값에 변경 사항이 있을 경우
  값이 유지되지 않고 가상 DOM의 변경 사항에 의해 덮어 씌어질 수 있다.

  따라서 실제 DOM 요소에 접근하기 위해
  React에서는 <a href="#RefHook">RefHook</a>을 제공한다.
  
  이 Hook은 실제 DOM의 특정 요소에 접근하지만 참조 관계를 유지하여 
  상태나 속성이 변경되어 가상 DOM이 다시 렌더링될 떄에도 
  값이 초기화되지(덮어씌어지지) 않고 유지된다.
</pre>

<br>
<hr>
<br>

## ☑️ JSX (JavaScript XML "eXtensible Markup Language")

JavaScript 코드 내에서 XML 또는 HTML과 같은 구문을 작성할 수 있게 해주는 React 문법. <br>
> JS의 개발속도가 브라우저의 개발속도보다 빨라 JSX는 브라우저가 읽지 못하기 때문에 <br>
Babel.js 와 같은 트랜스파일러를 이용하여 JS코드로 변환한다. <br>

<br>

## ⚡️ React의 return
<pre>
React의 컴포넌트는 return 값으로 JSX , React 요소를 반환한다. <br>

<pre class="highlight"><code>
// JSX
return &lt;h1&gt;Hello, world!&lt;/h1&gt;

// Babel.js 변환식
React.createElement("h1", null, "Hello, world!");
</code></pre>

이 반환값은 Babel이 createElement()를 호출하여 JS 요소로 변환해주며 <br>
가상 DOM을 생성하고 실제 DOM에 반영할 수 있게 해준다. <br>

➡️ 브라우저에 렌더링할 UI를 정의 <br>

</pre>

<br>

<pre>
JS, CSS 문법에서 ‘-’ snake-case으로 작성된 속성명을
camelCase로 재정의해두었다.
    ex1) text-align → textAlign
    ex2) <button onClick=””></button>
</pre>

<details>
<summary>☑️ HTMLAttributes</summary>
<li>기본 속성</li>
<div markdown="1">

```javascript
interface HTMLAttributes<T> extends AriaAttributes, DOMAttributes<T> {
  // React-specific Attributes
  defaultChecked?: boolean | undefined;
  defaultValue?: string | number | readonly string[] | undefined;
  suppressContentEditableWarning?: boolean | undefined;
  suppressHydrationWarning?: boolean | undefined;
  
  // Standard HTML Attributes
  accessKey?: string | undefined;
  autoCapitalize?: "off" | "none" | "on" | "sentences" | "words" | "characters" | undefined | (string & {});
  autoFocus?: boolean | undefined;
  className?: string | undefined;
  contentEditable?: Booleanish | "inherit" | "plaintext-only" | undefined;
  contextMenu?: string | undefined;
  dir?: string | undefined;
  draggable?: Booleanish | undefined;
  enterKeyHint?: "enter" | "done" | "go" | "next" | "previous" | "search" | "send" | undefined;
  hidden?: boolean | undefined;
  id?: string | undefined;
  lang?: string | undefined;
  nonce?: string | undefined;
  slot?: string | undefined;
  spellCheck?: Booleanish | undefined;
  style?: CSSProperties | undefined;
  tabIndex?: number | undefined;
  title?: string | undefined;
  translate?: "yes" | "no" | undefined;
  
  // Unknown
  radioGroup?: string | undefined; // <command>, <menuitem>
  
  // WAI-ARIA
  role?: AriaRole | undefined;
  
  // RDFa Attributes
  about?: string | undefined;
  content?: string | undefined;
  datatype?: string | undefined;
  inlist?: any;
  prefix?: string | undefined;
  property?: string | undefined;
  rel?: string | undefined;
  resource?: string | undefined;
  rev?: string | undefined;
  typeof?: string | undefined;
  vocab?: string | undefined;
  
  // Non-standard Attributes
  autoCorrect?: string | undefined;
  autoSave?: string | undefined;
  color?: string | undefined;
  itemProp?: string | undefined;
  itemScope?: boolean | undefined;
  itemType?: string | undefined;
  itemID?: string | undefined;
  itemRef?: string | undefined;
  results?: number | undefined;
  security?: string | undefined;
  unselectable?: "on" | "off" | undefined;
  
  // Living Standard
  /**
    * Hints at the type of data that might be entered by the user while editing the element or its contents
    * @see {@link https://html.spec.whatwg.org/multipage/interaction.html#input-modalities:-the-inputmode-attribute}
    */
  inputMode?: "none" | "text" | "tel" | "url" | "email" | "numeric" | "decimal" | "search" | undefined;
  /**
    * Specify that a standard HTML element should behave like a defined custom built-in element
    * @see {@link https://html.spec.whatwg.org/multipage/custom-elements.html#attr-is}
    */
  is?: string | undefined;
}
```
</div>
</details>

<br>
JS의 데이터에 접근(문법을 사용)하기 위해서는 {중괄호}로 작성한다.

```java
function example(javascript) {
	const js = "자바스크립트";
	return (
		<>
      {/* JSX 문법으로 작성해야 하는 영역 */}
      javascript : {javascript}
      js : {js}
		</>
	); 
	// javascript : <입력된 매개변수의 값>
	// js : 자바스크립트*
}
```
<br>

## JSX를 사용하는 이유
<pre>
  마크업 언어인 HTML의 문법 사이에 
  JS 로직을 자연스럽게 적용할 수 있다. (HTML에 친화적인 디자인)
  React 컴파일 과정에서 오류를 감지할 수 있다.
</pre>
<br>

⚡️ 컴포넌트 단위의 개발은<br>
&emsp;기능에 따라 로직을 분리하기 때문에<br>
&emsp;복잡한 문제를 작게 나누어서 해결할 수 있다.<br>

<br>

☑️ 컴포넌트는 캡슐화되고 필요에 따라 재사용이 가능하다.<br>
&emsp;또한 필요한 로직에 쉽게 결합할 수 있으며<br>
&emsp;export, import를 통해 기능을 확장할 수 있다.<br>

<br>

html 태그의 속성 (property)
```java
<img src="image.jpg" alt="IMAGE"></img>
```
src, alt 등과 같은 속성값을 자식 컴포넌트에 <br>
매개변수 형태로 전달하여 태그를 동적으로 관리, 제어한다. <br>


<details><!-- Props Start -->
<summary class='summary-title'>✅ Props (Properties)</summary>
<li>부모 컴포넌트의 데이터를 자식 컴포넌트로 전달할 때 <br>
&emsp;&ensp;사용되는 읽기 전용 데이터 (수정 불가)</li>
<li>부모 → 자식 방향으로만 데이터를 전달할 수 있다.</li>
<li>함수형 컴포넌트와 클래스 컴포넌트 모두 사용할 수 있다.<br>
&emsp;&ensp;사용방식에는 차이가 있다</li>
<br>

<details><!-- 클래스 컴포넌트 Start -->
<summary>클래스 컴포넌트</summary>
<li>매개변수로 props를 전달받지 않아도 <br>
&emsp;&ensp;this 키워드를 통해 props 변수에 접근할 수 있다.</li>

<div markdown="1">

```java
// 부모에서 자식 컴포넌트 호출
<EaseHee name={name} age={age}/>

// 
 class EaseHee extends Component {
	render() {
		return <>{this.props.name} : {this.props.age}</>
	}
}
```
</div>
</details><!-- 클래스 컴포넌트 End -->


<details>
<summary>함수형 컴포넌트</summary>
<li>매개변수로 props를 전달받아야 하고 <br>
&emsp;&ensp;{중괄호}를 통해 JS의 변수명으로 직접 전달 받을 수도 있다.</li>
<div markdown="1">

```java
const EaseHee = props => {
	return <>{props.name} : {props.age}</>
}

// 여기서 매개변수명은 자유롭게 설정할 수 있다. 한글도 가능하다
const EaseHee = 유기력 => {
	return <>{유기력.name} : {유기력.age}</>
}

// {변수명}으로 직접 접근 가능
const EaseHee = {age, name} => {
	return <>{name} : {age}<>
}
```
</div>
</details><!-- 함수형 컴포넌트 End -->
</details><!-- Props End -->


<details><!-- State Start -->
<summary class='summary-title'>✅ State</summary>
<li>컴포넌트 내부에서 컴포넌트의 상태를 관리하는 동적 데이터</li>
&emsp;props : 부모로부터 전달받은 데이터 “수정 불가” <br>
&emsp;state : 컴포넌트 내부 데이터 “수정 가능” <br>
<br>

<li>데이터가 변화하면 state가 업데이트되어 컴포넌트가 ReRendering된다.</li>

<details><!-- 클래스 컴포넌트 start -->
<summary class=''>클래스 컴포넌트</summary>
<li>변수의 값을 수정할 함수를 정의 & setState() 호출</li>
<li>this.state 를 통해 접근 가능</li>
<li>State HOOK 사용</li>
<li>함수 내 setState() 호출</li>

<details>
<summary class=''>코드 연습</summary>

<div markdown="1">

```java
import { Component, useState, useEffect } from "react";

import HookTest from "./mydir/HookTest";
import HookTest2 from "./mydir/HookTest2";

/***** 클래스에서 state를 사용하는 방법 *****/

class App extends Component {
  /**
    * props, state 의 데이터가 변경되면 
    *  가상 DOM이 실제 DOM의 데이터를 변경한다. 
    *  -> re-rendering "변경된 데이터만 수정"
    * 
    * state 상태 변수 
    *  : 컴포넌트 내부에서 사용(관리)하는 동적 데이터 
    *  JSON 타입으로 데이터를 저장
    */
  state = {
    // 클래스의 지역 변수 : state 상태 변수 (자바의 private member field 개념)
    count:0,
  }

  countUpdate(n) {
    /* Virtual DOM 이 Re-Rendering */
    this.setState({count : n}); // 이미 구현된 함수를 호출. (key : value)를 JSON 형식으로 전달한다.
  }

  render() {
    /* 참조 변수의 필드에 접근하여 데이터를 치환할 수 있다. */
    const that = this; /* 참조값을 치환 */
    const status = that.state;  
    const {state} = this; // {중괄호}를 이용하면 필드에 직접 접근하여 데이터를 반환할 수 있다.
    const state2 = this.state;
    const {count} = this.state;
    const c = this.state.count;

    console.log(state === state2); // true
    console.log(state == state2); // true
    
    return (
      <div>
        <h2>클래스형 컴포넌트 지역변수 state</h2>
        that.state.count : {that.state.count} <br></br>
        status.count : {status.count} <br></br> {/* 변수의 참조값을 치환하기 때문에 필드 멤버로 접근할 수 있다. (당연한 얘기) */}
        this.state.count : {this.state.count} <br></br>
        state.count : {state.count} <br></br>
        count : {count} <br></br>
        c : {c} <br></br>
        {console.log(c)}{/* { 여기는 그냥 자바스크립트다.. } */}
        <button /* JS의 onclick과 다르다! */ onClick={() => {
          /* { JS의 영역 } 에서는 {count} 가 아니라 count 로 바로 접근한다. */
          this.countUpdate(count+1); 
        }}>증가 1</button> <br></br>
        <button onClick={() => this.countUpdate(count + 2)} > + 2 </button>
        <br></br>
        <hr></hr>
        <HookTest/>
        <hr></hr>
        <HookTest2></HookTest2>
      </div>
    );
  }
}
```
</div>

</details>

<details>
<summary class=''>축약본</summary>

<div markdown="1">

```java
import { Component, useState, useEffect } from "react";

/***** 클래스에서 state를 사용하는 방법 *****/
class App extends Component {
  state = {count:0}
  /* this 키워드를 이용하여 setState() 호출 */
  const countUpdate = n => this.setState({count : n});

  render() {
    const {count} = this.state;

    return (
      <div>
        count : {count} <br></br>
        <button onClick={() => this.countUpdate(count + 1)}> + 1</button>
      </div>
    );
  }
}
```
</div>

</details>
</details><!-- 클래스 컴포넌트 End -->


<details><!-- 함수형 컴포넌트 start -->
<summary class=''>함수형 컴포넌트</summary>
<li>useState() 를 이용하여 함수 내 지역 변수를 선언하는 개념</li>
<div markdown="1">

```java
/* useState */
const [count, setCount] = useState(0);
const countUpdate = 매개변수 => setCount(count + 매개변수);
```
</div>

<details>
<summary class=''>코드연습</summary>

<div markdown="1">

```java
/***** 함수 타입에서 state를 사용하는 방법 *****/

const Application = () => {
  /* useState */
  const [count, setCount] = useState(0);
  const countUpdate = 매개변수 => setCount(count + 매개변수);

  /* useEffect : 변화가 생기면 호출되는 함수 */
  useEffect(() => {
    document.title = `클릭 횟수 : ${count}회`; // 페이지의 제목을 동적으로 변경할 수 있다.
    let 지역변수 = 1; // 한글도 되네... 
    console.log("========== " + (지역변수 + count) + " =========="); // ()로 묶지 않으면 기본데이터와 참조변수가 서로 연산되지 않기 때문에 문자열로 나열된다. 1 + 1 = 11
  }, [count]); // 변화를 추적할 변수를 배열로 두번째 인자에 전달

  return (
    <div> {/* className 을 주려면 <>로 하면 안 된다 (당연한 얘기) */}
      number : {count} &nbsp;
      <button onClick={() => countUpdate(1)}>증가 1</button>
      <br></br>
      <hr></hr>
      <HookTest/>
      <hr></hr>
      <HookTest2></HookTest2>
    </div>
  );
}
```
</div>
</details>

</details><!-- 함수형 컴포넌트 End -->
</details><!-- State End -->

<br>
<hr>
<br>



<details>
<summary>배포</summary>

<div markdown="1">

&emsp; 1. 서버 주소 입력 
```
// package.json
”homepage” : “http://Domain:Port/API/”
```


&emsp; 2. 프로젝트 빌드
```
%> npm run build
%> npm install -g serve
```

&emsp; 3. 서버 프로젝트 내 주입 후 실행
<br>
![](/assets/image/2024-11-14_React_Distribution.png)
  

&emsp; 4. 이후 스프링 프로젝트 배포 (jar)
```
%> ./gradlew build

%> cd build/lib

%> java -jar 프로젝트명-0.0.1-SNAPSHOT.jar
```
</div>
</details>


<br>
<hr>
<br>


---

참고 자료 및 참고 사이트 <br>
[https://cafe.daum.net/flowlife](https://cafe.daum.net/flowlife/QbpR/68) <br>
[생활코딩 React 리액트 프로그래밍, 이고잉](https://wikibook.co.kr/react-rev/) <br>
[https://react.dev/](https://react.dev/reference/react)
