---
layout: post
title: "React_03_Hooks_02_Memo & Callback"
date: 2024-11-16
categories: blog
category: React
---

<br>

---

# ⚡️ Memoization
<pre>
프로그래밍에서 성능 최적화를 위해 자주 사용되는 기법

컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 
이전에 계산한 값을 메모리에 저장함으로써 
동일한 계산의 반복 수행을 제거하여 
프로그램 실행 속도를 빠르게 하는 기술.

동적 계획법의 핵심 기술

- 동적 계획법
  복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법.
  부분 문제 반복, 최적 부분 구조를 가지고 있는 알고리즘을 일반적인 방법에 비해 적은 시간 내에 풀 수 있다.
참고 : <a href="https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98">위키백과</a>

- React에서의 메모이제이션
  React.memo, useMemo, useCallback 등을 사용
  컴포넌트의 렌더링 성능을 최적화하는 데 활용

리액트에서 부모 컴포넌트의 
Props 혹은 State가 변경되어 리렌더링되면
부모 컴포넌트에 속한 모든 자식 컴포넌트는 전부 렌더링 된다.

하지만 부모 컴포넌트에서 자식 컴포넌트로 
전달하는 props가 바뀌지 않았다면, 
해당 자식 컴포넌트를 리렌더링 하지 않아도 된다.

컴포넌트에서 리렌더링이 필요한 상황에서만 
해주도록 설정을 할 수 있는데 
이때 사용하는 함수가 바로 React.memo 함수다
</pre>


<details><!-- Memo Start -->
<summary class='summary-title' id="memo">Memo</summary>
<li>Memoization</li>
<li>Memo Hook</li>
<li>Callback Hook</li>
<br>
<pre>
- Memoization
  프로그래밍에서 성능 최적화를 위해 자주 사용되는 기법
  컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 
  이전에 계산한 값을 메모리에 저장함으로써 
  동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다. 
  동적 계획법의 핵심 기술

- 동적 계획법
  복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법.
  부분 문제 반복, 최적 부분 구조를 가지고 있는 알고리즘을 일반적인 방법에 비해 적은 시간 내에 풀 수 있다.
</pre>
<br>
<pre class='highlight'><code>
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
</code></pre>
<pre>
  - Component
    memoize 대상 컴포넌트.
    memo가 컴포넌트를 수정하지 않고 memoized 컴포넌트를 반환한다.
      
  - arePropsEqual (optional)
    컴포넌트의 이전 props(prevProps)와 새로운 props(nextProps) 두 가지를 인수로 받는 함수.
    prevProps === nextProps
      동일 결과 렌더링 & 
      동작 방식의 동일 여부 판단 후 
      true / false 반환
    prevProps ≠ nextProps
      false 반환

참고 사이트 : <a href="https://ko.react.dev/reference/react/memo">[https://ko.react.dev/reference/]</a>

- useMemo : 메모이제이션된 값을 반환
- useCallback : 메모이제이션 된 콜백을 반환

useMemo와 유사하게 이용. '함수'에 적용
    
의존성이 변경되었을때만 변경. 

✅ 특정 함수를 새로 만들지 않고 재사용할 수 있다.
</pre>

</details><!-- Memo End -->
<br/>

<details><!-- Memo Hook Start -->
<summary class='summary-title'>Memo Hook</summary>

<details><!-- useMemo() Start -->
<summary>useMemo()</summary>
<li>계산 결과를 메모이제이션.</li>
<li>의존성 배열이 변경되지 않는 한 계산을 다시 수행하지 않는다.</li>
<li>반환 값 : 계산 결과 (값)</li>

<pre class='highlight'><code>
import { memo } from "react";

const Child = (/* props */{name, age}) => {
    console.log("와우 자녀 나이 변경 (리렌더링)");
    return (
        <div style={boxStyle}>
            <h3>🙂 자녀 1</h3>
            <p>나이 : 8</p>
            <br></br>
            <h3>🙃 자녀 2</h3>
            <p>이름 : {name}</p>
            <p>나이 : {age}</p>
        </div>
    )
}

/* memo : 부모 컴포넌트가 리렌더링될 때 자식 컴포넌트의 속성, 상태에 변화가 없다면 렌더링되지 않도록 관리 */
export default memo(Child);


/*** 부모 컴포넌트 ***/
import { useState } from "react";
import Child from "./Child.js";

function Father() {
  // TODO 1118 React _ React.memo & Memoization
  const [fatherAge, setFatherAge] = useState(34);
  const [childAge, setChildAge] = useState(3);

  const changeFatherAge = () => {
    setFatherAge(fatherAge + 1);
  };
  
  const changeChildAge = () => {
    setChildAge(childAge + 1);
  };
  
  console.log("아빠 나이 변경 (리렌더링)");
  
  return (
    <div style={boxStyle}>
      <h2>😎 아빠</h2>
      <span>나이 : {fatherAge}</span>&emsp;<button type="button" onClick={changeFatherAge}>아빠 나이 변경</button>
      <br></br>
      <br></br>
      <Child name={"유기력"} age={childAge}></Child>
      <button type="button" onClick={changeChildAge}>자녀 나이 변경</button>
    </div>
  );
}

export default Father;
</code></pre>

</details><!-- useMemo() End -->

</details><!-- Memo Hook End -->




<details><!-- Callback Hook Start -->
<summary class='summary-title'>Callback Hook</summary>

<details><!-- useCallback() Start -->
<summary>useCallback()</summary>
<li>함수를 메모이제이션.</li>
<li>의존성 배열이 변경되지 않는 한 <br>
⚠️ 해당 함수의 참조가 변경되지 않도록 한다. <br>
✅ 특정 함수를 새로 만들지 않고 재사용할 수 있다.</li>
<li>반환 값 : 함수 자체를 반환</li>

<pre><code>
const ClickHandler = () => {
	const [count, setCount] = useState(0);
	
  // 처리 함수를 인자로 전달한 경우
	const myCallback = useCallback(() => {
		setCount(prev => prev + 1); 
	}, []); // 처음 렌더될 떄 함수를 생성한 후 변경하지 않음
	
  // 위와 동일. 상태변수를 인자로 전달하고 의존성배열에 상태변수를 전달한 경우
  const myCallback = useCallback(() => {
    setCount(count + 1);
  }, [count]); // count 변수의 값이 변경될 때마다 함수를 재생성


	return <button type="button" onClick={myCallback}>EaseHee</button>	
}
</code></pre>

</details><!-- useCallback() End -->

</details><!-- Callback Hook End -->


<br>
<br>

<details><!-- 의존성 배열 start -->
<summary class='summary-title'>⚡️의존성 배열 (Dependecies)</summary>
<li>useEffect(), useCallback() 등</li>
<li>(처리 함수, 의존성 배열) 을 인자로 전달하여 <br>
&emsp;컴포넌트의 생명주기를 관리할 수 있다</li>

<pre>
💡 의존성 배열
  : 함수에서 state 변수를 사용할 경우 데이터가 변경되어 리렌더링 될 경우 함수를 재생성하게 된다.

  1. 의존성 배열을 전달하지 않는 경우
  2. 의존성 배열을 속이 비어있는 상태로 전달한 경우
  3. 의존성 배열에 참조할 데이터를 전달할 경우
</pre>

<details>
<summary>useCallback 예시</summary>
&emsp;모두 동일

<div markdown="1">

```jsx
const addVisit = useCallback(() => {
  ...
  setVisits(visits.concat(formData)); // 직접 참조 시 의존성 배열에 추가
  setNextId(nextId + 1); // 직접 참조 시 의존성 배열에 추가
  setFormData({});
}, [formData, nextId, visits]) // 확인
```

```jsx
  ...
  setVisits(prev => prev.concat(formData));
  setNextId(nextId + 1);
  setFormData({});
}, [formData, nextId]) // 확인
```

```jsx
  ...
  setVisits(prev => prev.concat(formData));
  setNextId(prev => prev + 1);
  setFormData({});
}, [formData]) // 확인
```
</div>
</details>
    

</details><!-- 의존성 배열 end -->

---

참고 자료 및 참고 사이트 <br>
[https://cafe.daum.net/flowlife](https://cafe.daum.net/flowlife/QbpR/68) <br>
[생활코딩 React 리액트 프로그래밍, 이고잉](https://wikibook.co.kr/react-rev/) <br>
[https://react.dev/](https://react.dev/reference/react)
