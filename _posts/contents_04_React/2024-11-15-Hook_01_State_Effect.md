---
layout: single
title: "React_02_Hooks_01_State & Effect"
date: 2024-11-15
categories:
  - React
tags:
  - React
  - Hook
  - State
  - Effect
---

<br>

---


# HOOK
<pre>
  함수형 컴포넌트는 로직이 종료되면 메모리 상에서 사라지기 때문에
  상태 정보에 대한 접근과 라이프 사이클을 구현할 수 없었다.
  그래서 지역변수를 사용하거나 라이프 사이클을 구현하기 위해서는
  클래스 컴포넌트를 선언해야 했다.
</pre>
<br>

⚡️ React Hooks의 등장으로 <br>
&emsp;함수형 컴포넌트에서 상태 값에 대한 접근이 가능해지고 <br>
&emsp;자식 요소에 접근할 수 있게 되었다.<br>
&emsp;클래스에서는 사용할 수 없다. “함수에서만 사용(접근) 가능”<br>
    
```java
import React, { useState, useEffect } from 'react';
// useState : 지역변수 선언을 위한 Hook
// useEffect : 부수 효과를 수행하기 위한 Hook

const Counter = () => {
  // 상태변수 count와 수정하기 위한 함수 setCount()를 콜백으로 설정
  const [count, setCount] = useState(0);
  // 상태변수 count의 값을 수정하기 위해서는 setCount() 함수를 사용한다.
}

// 특정 조건에 따라 함수가 호출되도록 설정 (의존성 배열로 상태 변수를 설정하여 변경 시 함수를 재생성한다.)
const Effect (() => {
  document.title = `클릭 횟수 : ${count}회`;
}, [count]);
```

<br>

<h2>주요 내장 HOOK</h2>

<details>
<summary class='summary-title'>State Hook</summary>
<li>컴포넌트 데이터의 상태에 접근하여 데이터를 수정할 수 있다</li>

<details>
<summary class=''>useState()</summary>
<pre><code>
import { useState } from 'react';

function MyComponent() {
  // setState() 변수를 여러 개 선언
  const [age, setAge] = useState(28);
  const [name, setName] = useState('Taylor');
  const [todos, setTodos] = useState(() => createTodos());
  // ...
</code></pre>

배열 구조 분해 (Destructuring)에 의해 <br>
useState로 호출된 state 변수들을 다른 변수명으로 할당

</details><!-- useState() End -->
</details><!-- State Hook End -->


<details><!-- Effect Hook Start -->
<summary class='summary-title'>Effect Hook</summary>
<pre>
입력값과 관계없이 항상 같은 결과를 반환할 때 순수 함수라고 하고,
이 경우는 Side Effect (부작용)이 발생하지 않는다.

그런데 props로 전달되는 데이터가 변경되거나 
컴포넌트의 상태가 변경되어 side effect가 발생하는 경우 
Effect Hook을 이용하여 side effect를 처리한다. 

(참고) 라이프 사이클 메서드
1. componentDidMount
  컴포넌트가 트리에 삽입된 후 호출 (등록)
2. componentDidUpdate
  컴포넌트 데이터가 변경된 후 호출 (수정)
3. componentWillUnmount
  컴포넌트가 제거 되기 전 호출 (삭제)
</pre>   

<br>

<details>
<summary>useEffect(setup, dependencies?)</summary>
<li>setup : 처리함수</li>
<li>dependecies : 변경 감지 데이터 배열</li>
<li>return : 컴포넌트 언마운트 시 실행 함수</li>
<br>
<li>인자로 전달되는 데이터 배열의 상태에 따라 처리 방식이 3가지로 구분된다.</li>
<div markdown="1">
<pre>
  1. 배열을 전달하지 않은 경우
    렌더링과 이후 모든 업데이트 과정에서 effect를 감지 하여 함수 호출
    ex) logging
      
  2. 배열에 데이터가 없이 전달된 경우.
    처음 렌더링될 때에만 함수 호출.
    ex) 초기 데이터 fetch 등
</pre>  

```java
useEffect(() => {
    fetch('https://api.easehee.com/data')
    .then(response => response.json())
    .then(data => setData(data));
}, []); 
```
<pre>
  3. 배열에 데이터가 전달된 경우.
    배열에 특정 상태나 props를 의존성으로 전달.
    해당 값이 변경될 때에만 함수 호출.
    ex) 데이터 필터링, API 호출 등
</pre>

```java
useEffect(() => {
  document.title = `클릭 횟수 : ${count}회`; // 페이지의 제목을 동적으로 변경할 수 있다.
  let 지역변수 = 1; 
  console.log("========== " + (지역변수 + count) + " =========="); 
  // ()로 묶지 않으면 기본 데이터타입과 참조변수가 서로 연산되지 않기 때문에 문자열이 나열된다. 1 + 1 = 11
}, [count]); // 변화를 감지할 변수를 배열에 담아 두번째 인자로 전달
```
    
- 배열 인자에 따른 출력 Effect Hook 사용 예 3가지
    
```java
import React, { useState, useEffect } from 'react';

function LoggingComponent() {
  // useState() -> 지역변수 선언, 변경함수 설정, 변수 초기화
  const [inputValue, setInputValue] = useState('');

  // 1> 컴포넌트가 렌더될 때마다 함수 호출
  useEffect(() => {
    console.log('와우');
  });
  
  // 2> 컴포넌트가 처음 렌더될 때에만 함수 호출
  useEffect(() => {
    console.log('[LoggingComponent Rendered] 와우');
  }, []);

  // 3> state 변수 inputValue의 상태가 변경될 때에만 함수를 호출
  useEffect(() => {
    console.log(`[입력값 변경] Value : ${inputValue}`);
  }, [inputValue]);

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={ event => setInputValue(event.target.value)}
        placeholder="값 수정"
      />
      <p>Value : {inputValue}</p>
    </div>
  );
}

export default LoggingComponent;
```
</div>

<br>

✅ useEffect & LifeCycle
<br>
<div markdown="1">

```java
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom(roomId) {
	// URL 설정
  const [serverUrl, setServerUrl] = useState('https://localhost:3000');

  useEffect(() => { /* componentDidMount */
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
		
    return () => { /* return => componentWillUnmount */
      connection.disconnect();
    };
  }, [serverUrl, roomId]);
  
  // ...
  
}
```
> 출처 : [https://react.dev/](https://react.dev/reference/react/useEffect)
</div>


</details><!-- useEffect() End -->
</details><!-- Effect Hook End -->

<details><!-- Ref Hook Start -->
<summary class='summary-title' id="RefHook">Ref Hook</summary>
<li>실제 DOM의 특정 요소에 접근하여 참조하는 방식</li>
<li>JS의 <i>document.querySelector()</i>를 대신하여 사용한다.</li>
<li>상태나 속성 값이 변경되어 컴포넌트가 다시 렌더링 돼도 값이 유지된다.</li>

<details><!-- useRef() start -->
<summary>useRef()</summary>

<pre class='highlight'><code>
import { useRef, useEffect } from 'react';

const ExampleComponent = () => {
  /**
   * DOM요소가 처음에는 없기 때문에 초기값을 null로 설정하지만, 
   * 이후 참조 관계가 생기면 .current 속성에 의해 DOM 요소를 참조하게 된다.
   */
  const inputRef = useRef(null);

  useEffect(() => {
    // React가 제어하는 범위 내에서 DOM에 접근한다.
    inputRef.current.focus();
  }, []); // 화면이 처음 렌더될 때 input 태그에 포커싱하도록 설정

  return <input ref={inputRef} type="text" />; // 참조해야 하는 DOM 요소 "ref 속성을 부여하여 참조 관계를 유지한다"
};
</code></pre>

</details><!-- useRef() End -->
</details><!-- Ref Hook End -->



<br>
<hr>
<br>


# ✅ React LifeCycle

- 모식도

![](/assets/image/2024-11-15-React_LifeCycle.png)

출처 : [https://velog.io/@minbr0ther/React.js-리액트-라이프사이클life-cycle-순서-역할](https://velog.io/@minbr0ther/React.js-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4life-cycle-%EC%88%9C%EC%84%9C-%EC%97%AD%ED%95%A0)

<br/>


<details><!-- 클래스 컴포넌트 start -->
<summary>클래스 컴포넌트</summary>
<li>라이프 사이클 메서드를 직접 호출하여 생명주기 관리</li>
<br>

<pre class="rec">
===== 클래스 컴포넌트의 생명주기 관리 =====
    mount : DOM이 생성되고 결과가 출력되는 것을 의미

*** 마운트 호출 순서 ***
    1> constructor() : 컴포넌트를 생성
    2> getDerivedStateProps()
    3> render()
    4> componentDidMount()

*** 업데이트 호출 순서 ***
    1> static getDerivedStateFromProps()
    2> shouldComponentUpdate()
    3> render()
    4> getSnapshowBeforeUpdate()
    5> componentDidUpdate()

*** 마운트 해제 ***
    componentWillUnmount()
</pre>

<br/>

      
<br>

<pre class='highlight'><code>
class Clock2 extends React.Component {
    /* 생성자를 통해 상위 클래스에 props 전달 */
    constructor (props) {
        super(props);
        /* *** this.state는 생성자에서만 값을 줄 수 있다. *** */
        this.state = {date: new Date()}; // date 라는 key 
        /* 생성자 호출 시 멤버 변수에 날짜를 저장 : 화면이 리렌더링 되어도 변경되지 않는다. */
        this.date = new Date().toLocaleTimeString();
    }

    /* 시간 출력용 메서드 (클래스 내부의 함수는 메서드) */
    showTime = () => {
        // 현재 시간으로 state 변수 갱신
        this.setState({date:new Date()});
    }
    
    /**
     * *** Life Cycle Callback Methods *** 
     *  클래스 컴포넌트에서만 사용할 수 있다. 
     */

    /* 최초 컴포넌트 렌더링이 완료되면 시스템이 호출 (콜백) */
    componentDidMount = () => {
        /* setInterval(함수, 지연시간ms) : 지연시간ms 간격으로 함수를 호출 */
        this.timerId = setInterval(this.showTime, 1000);
    }

    /* 컴포넌트가 화면에서 사라지기 직전에 호출 (콜백) "마무리 작업에서 사용" _ 자바의 destroy와 유사 */
    componentWillUnmount() {
        /* 타이머 해제 : clearInterval() 호출 = 인자로 timer 변수 전달 */
        clearInterval(this.timerId); // 생명 주기 관리 
    }

    render() {
        return (
            <>
                &lt;div&gt;
                    &lt;h1&gt;Clock2 (클래스 컴포넌트)&lt;/h1&gt;
                    &lt;h2&gt;this.date : {this.date}&lt;/h2&gt;{/* 고정 시간 (처음 렌더링될 때의 시각에 멈춰있다) */}
                    &lt;h2&gt;new Date() : {new Date().toLocaleTimeString()}&lt;/h2&gt;{/* 아래 state 변수가 변경될 때마다 함께 리렌더링된다 */}
                    &lt;h2&gt;this.state.date : {this.state.date.toLocaleTimeString()}&lt;/h2&gt;{/* this.state.date 변수 : setInterval() 설정 */}
                &lt;/div&gt;
            </>
        )
    }
}

export default Clock2;
</code></pre>

</details><!-- 클래스 컴포넌트 end -->


<details><!-- 함수형 컴포넌트 start -->
<summary>함수형 컴포넌트</summary>
<li>Effect Hook을 이용하여 생명주기 관리</li>

<details><!-- componentDidMount start -->
<summary>componentDidMount </summary>
<li>useEffect(func, []);</li>

주로 DOM을 사용해야 하는 외부 라이브러리 연동, <br/>
해당 컴포넌트에서 필요로하는 데이터를 ajax로 요청, 등의 행위 <br/>
  
- useEffect() 의 두번째 인자 (의존성 배열)을 통해 관리할 수 있다.
  
<pre class='highlight'><code>
useEffect(()=>{
  console.log("componentDidMount");     
},[])
</code></pre>

</details><!-- componentDidMount end -->
      


<details><!-- componentDidUpdate start -->
<summary>componentDidUpdate </summary>
<li>useEffect(func, [prop]);</li>

의존성 배열이 변할때만 useEffect 실행
      
<pre class='highlight'><code>
useEffect(()=>{
  console.log("componentDidUpdate");     
},[variables]) // variables가 갱신될  때 호출
</code></pre>

</details><!-- componentDidUpdate end -->
      


<details><!-- componentWillUnmount start -->
<summary>componentWillUnmount</summary>
<li>useEffect(func);</li>
<li>컴포넌트가 화면에서 사라지기 직전에 호출</li>
<li>return 시 반환 값으로 설정한 함수를 호출</li>

<pre class='highlight'><code>
useEffect(() => {
    console.log("componentDidMount"); 
    return {console.log("componentWillUnmount")}}
}, []);
</code></pre>

</details><!-- componentWillUnmount end -->

<br/>
<pre class='highlight'><code>
const Clock3 = () => {
    /* state 변수 date 초기화  */
    const [date, setDate] = useState(new Date());
    /**
     * Life Cycle 관련 Hook 
     *  : Effect Hook -> useEffect() 
     *  "componentDidMount, componentDidUpdate, componentWillUnmount" 
     */
    useEffect(() => {
        /* interval 설정을 위한 타이머  */
        const timerId = setInterval(showTime, 1000);

        /* *** return : componentWillUnmount에 해당된다 *** */
        return () => {
            /* interval이 설정된 객체 초기화 = 타이머 */
            clearInterval(timerId);
        }
        
    }, []) // 의존성 배열이 비어있으면 1회만 호출된다.

    /* setInterval 에 콜백으로 전달할 함수 : state 변수 date에 값 치환 */
    const showTime = () => setDate(new Date());
    
    return (
        &lt;div&gt;
            &lt;h1&gt;Clock3 (함수형 컴포넌트)&lt;/h1&gt;
            {/* &lt;h2&gt;this.date : {date}&lt;/h2&gt;고정 시간 (처음 렌더링될 때의 시각에 멈춰있다) */}
            &lt;h2&gt;new Date() : {new Date().toLocaleTimeString()}&lt;/h2&gt;{/* 아래 state 변수가 변경될 때마다 함께 리렌더링된다 */}
            &lt;h2&gt;date : {date.toLocaleTimeString()}&lt;/h2&gt;{/* state 변수 date : setInterval() 설정 */}
        &lt;/div&gt;
    )
}

export default Clock3;
</code></pre>
</details><!-- 함수형 컴포넌트 end -->


---

참고 자료 및 참고 사이트 <br>
[https://cafe.daum.net/flowlife](https://cafe.daum.net/flowlife/QbpR/68) <br>
[생활코딩 React 리액트 프로그래밍, 이고잉](https://wikibook.co.kr/react-rev/) <br>
[https://react.dev/](https://react.dev/reference/react)
