---
layout: post
title: "Thymeleaf와 템플릿 엔진"
date: 2024-10-19
categories: blog
category: Web
---

<br>

---
# Thymeleaf

> 타임리프를 이해하기 위해서는 템플릿 엔진이라는 개념을 이해해야 한다.

Template Engine
: 비즈니스 로직과 프레젠테이션 로직을 분리하기 위한 수단<br>
Controller에서 View로 데이터를 전송<br>
Servlet Code로 변환되지 않는다<br>
> ex) mustache, thymeleaf 등<br>
<i> jsp는 tomcat(서블릿 컨테이너)에 의해 컴파일되어 서블릿 코드로 변환된다.</i>

<br>

<h3>클라이언트 측 Template Engine</h3>
(CSR : Client Side Rendering) 방식 <br>
React, Vue, Angular.js 등  <br>
서버에서 데이터를 전달받아 웹 화면을 꾸며주는 라이브러리 혹은 프레임워크 <br>
SPA(Single Page Application)화면 : 비동기 처리(Ajax)등 <br>

<br>

<h3>서버 측 Template Engine</h3>
(SSR : Server Side Rendering) 방식 <br>
프레젠테이션 게층에서 로직을 쉽게 표현 <br>
개발의 유연성 향상, 유지 보수 효율 향상 <br>
템플릿 양식과 데이터를 이용하여 html을 생성하고 브라우저에 전달 <br>

<br>

<style>
    .exp th {
        text-align: center;
    }
    .exp td:Nth-of-type(1) {
        text-align: end;
    }
    .exp td:Nth-of-type(2) {
        text-align: center;
    }
    table {
        font-size: 1.1rem;
    }
    .desc {
        font-size: 1.2rem;
    }
</style>

<table class="exp">
    <thead>
        <tr>
            <th>표현식</th><th>설명</th><th>예시</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>@{ … }</td>
            <td>URL 링크 표현식</td>
            <td>
                &lt; ... th:href=”@{/css/bootstrap.min.css}” &gt;<br>
                &emsp;CSR방식의 페이지 요청 및 참조할 주소를 입력할 수 있다. <br>
                &emsp;thymeleaf가 적용되는 경우에만 유효하다. (당연한 얘기) <br>
            </td>
        </tr>
        <tr>
            <td>| ... |</td>
            <td>literal 대체</td>
            <td>
                &lt; ... th:text="|Hello ${user.name}!|" &gt; <br>
                &lt; ... th:text="'Hello '+${user.name}+'!'" &gt; 과 동일 <br>
                &emsp;JavaScript의 `(백틱)과 유사 <br>
            </td>
        </tr>
        <tr>
            <td>${ ... }</td>
            <td>
                변수 <br>
            </td>
            <td>
                &lt; ... th:text=${user.name} &gt;<br>
                &emsp;서버로부터 전달받은 데이터에 접근할 수 있다.<br>
                &emsp;데이터가 참조변수인 경우 참조값을 반환 (당연한 얘기) <br>
            </td>
        </tr>
        <tr>
            <td>*{ ... }</td>
            <td>선택 변수</td>
            <td>
                &lt; ... th:object="${items}" th:text="*${price}" &gt;<br>
                &emsp;object로 설정한 변수(items)의 속성에 접근할 수 있다. <br>
                &emsp;*${price} = ${items.price}
            </td>
        </tr>
        <tr>
            <td>#{ ... }</td>
            <td>
                메시지, properties 같은 외부 자원에서 <br>
                코드에 해당하는 문자열을 반환
            </td>
            <td>
                &lt; ... th:text="#numbers.formatInteger(data, 3, 'COMMA')" &gt;<br>
                &emsp;정수를 3자리 마다 , 로 구분 <br>
                &lt; ... th:text="#numbers.sequence(0, 100, 2)""&gt; <br>
                &emsp;0부터 100까지 2씩 증가
                <hr>
                &lt; ... th:text="#temporals.format(strDt, 'yyyy.MM.dd HH:mm:ss')" &gt;<br>
                &emsp;문자열 형식의 날짜를 Date 형식으로 변환 <br>
                &lt; ... th:text="#dates.format(item.regDt, 'yyyy.MM.dd HH:mm:ss')" &gt;<br>
                &emsp;Date형식을 Date 형식으로 변환 (패턴 부여)
            </td>
        </tr>
        <tr>
            <td>[[ ... ]]</td>
            <td>
                html 엘리먼트의 컨텐츠에 직접 작성 <br>
                타임리프가 적용되지 않을 경우 대체할 컨텐츠는 없다. 
            </td>
            <td>
                &lt;span&gt;[[${item.price}]]&lt;/span&gt;
            </td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
</table>

<br>
<hr>
<br>


## th:예약어

<br>



<table markdown="1" class="desc">
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
    <thead>
        <tr>
            <th>예약어</th><th>설명</th><th>예시</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>th:text</td>
            <td>
                텍스트 형식으로 데이터를 화면에 출력<br>
                문자열은 '작은 따옴표'로 묶어줘야 한다.
            </td>
            <td>
                <code>
                &lt;span th:text="${data}"&gt;<br>
                &lt;span th:text="'화면에 출력'"&gt;
                </code>
            </td>
        </tr>
        <tr>
            <td>th:with</td>
            <td>지역 변수 선언 (변수 형태의 값을 재정의)</td>
            <td>
                <code>
                    &lt;div th:with="no=${number}" th:text="${no}"&gt;<br>
                    &emsp;<i>no라는 지역변수에 매개변수 number를 저장하여 문자열로 출력</i>
                </code>
            </td>
        </tr>
        <tr>
            <td>th:object</td>
            <td>컨트롤러와 뷰(화면) 사이의 DTO 객체</td>
            <td>
                <code>
                &lt;div th:object="${object}"&gt;<br>
                &emsp;&lt;span th:text="*${name}"&gt;<br>
                </code>
            </td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>th:if</td>
            <td>조건이 참인 경우 컨텐츠 노출</td>
            <td>
                <code>
                    &lt;div th:if="true"&gt;출력되는 메시지&lt;/div&gt;<br>
                    &lt;div th:if="false"&gt;출력되지 않는 메시지&lt;/div&gt;
                </code>
            </td>
        </tr>
        <tr>
            <td>th:unless</td>
            <td>조건이 거짓인 경우 컨텐츠 노출</td>
            <td>
                <code>
                    &lt;div th:unless="true"&gt;출력되지 않는 메시지&lt;/div&gt;<br>
                    &lt;div th:unless="false"&gt;출력되는 메시지&lt;/div&gt;
                </code>
            </td>
        </tr>
        <tr>
            <td>th:switch<br>th:case</td>
            <td>조건이 case에 해당하는 경우 컨텐츠 노출</td>
            <td>
                <code>
                    &lt;div th:switch="${number.even}"&gt;<br>
                    &emsp;&lt;span th:case="true"&gt;짝수&lt;/span&gt;<br>
                    &emsp;&lt;span th:case="false"&gt;홀수&lt;/span&gt;<br>
                    &emsp;&lt;span th:case="*"&gt;그 외 (사실 없음)&lt;/span&gt;<br>
                    &lt;/div&gt;
                </code>
            </td>
        </tr>
        <tr>
            <td>th:each</td>
            <td>
                반복 처리를 수행하는 예약어로서 <br>
                &emsp;Iterable 구현 객체 <br>
                &emsp;Map의 EntrySet, KeySet, Values <br>
                &emsp;배열 등과 같은 객체가 대상이 된다. <br>
                cf. 변수명Stat으로 객체의 상태 정보에 접근할 수 있다.
                <hr>
                &emsp;<i>2번째 인덱스에 변수를 선언할 경우 <br>
                &emsp;객체의 상태에 관한 정보가 저장된다.</i> <br>
                &emsp;&emsp;index: 0~ <br>
                &emsp;&emsp;count: 1~ <br>
                &emsp;&emsp;size, even, odd, first, last   <br>
            </td>
            <td>
                <code>
                    &lt;tr th:each="item : ${list}" th:object="${item}"&gt; <br>
                    &emsp;&lt;td th:text="*{no}"&gt;&lt;/td&gt; <br>
                    &emsp;&lt;td th:text="*{name}"&gt;&lt;/td&gt; <br>
                    &emsp;&lt;td th:text="{itemStat.index}"&gt;&lt;/td&gt; <br>
                    &lt;/tr&gt; <br>
                </code>
                <hr>
                <code>
                    &lt;tr th:each="item, <strong>status</strong> : ${list}"&gt; <br>
                    &emsp;&lt;td th:text="${status.index}"&gt;&lt;/td&gt; <br>
                    &emsp;&lt;td th:text="${status.count}"&gt;&lt;/td&gt; <br>
                    &emsp;&lt;td th:text="${status.size}"&gt;&lt;/td&gt; = 
                    &emsp;&lt;td th:text="${list.size}"&gt;&lt;/td&gt; <br>
                    &lt;/tr&gt; <br>
                </code>
            </td>
        </tr>
    </tbody>
        <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>th:href</td>
            <td>
                참조할 경로의 주소를 작성할 수 있다. <br>
                경로의 경우 @{...} 표현식으로 작성한다.
            </td>
            <td>
                &lt;a th:href="@{/templates/list}"&gt;list 페이지 요청&lt;/a&gt; <br>
                &emsp;GET방식으로 요청 (당연한 얘기)
            </td>
        </tr>
        <tr>
            <td>
                th:action <br>
                th:method <br>
            </td>
            <td>
                &lt;form&gt;의 action, method 속성과 동일 <br>
                서버로의 요청명, 요청방식(기본값 GET)을 작성할 수 있다. <br>
                submit을 통한 서버로 데이터 전송 및 요청 <br>
            </td>
            <td>
                &lt;form th:action="@{/templates/list}" th:method="post"&gt; <br>
                &emsp;list 페이지 요청 <br>
                &lt;/form&gt;
            </td>
        </tr>
        <tr>
            <td>th:field</td>
            <td>
                th:object에 선언한 객체의 멤버 변수를 매핑 <br>
                &lt;form&gt;내 &lt;input&gt;, &lt;textarea&gt;의 <br>
                id, name, value 값을 자동으로 입력
            </td>
            <td>
                <div style="display: flex; flexflow: nowrap row;">
                    <code>
                        &lt;form th:object="${post}"&gt;<br>
                        &emsp;&lt;input th:field="*{title}"&gt;&lt;br&gt;<br>
                        &emsp;&lt;textarea th:field="*{content}"&gt;&lt;/textarea&gt;<br>
                        &lt;/form&gt;
                    </code>
                    &emsp;&emsp;&emsp;<br>
                    &emsp;→&emsp;
                    <code>
                        &lt;form&gt;<br>
                        &emsp;&lt;input id="title" name="title" value="제목"&gt;&lt;br&gt;<br>
                        &emsp;&lt;textarea id="content" name="content"&gt;내용&lt;/textarea&gt;<br>
                        &lt;/form&gt;
                    </code>
                </div>
            </td>
        </tr>
        <tr>
            <td>th:checked</td>
            <td>
                checkbox, radio에서 조건이 "true"에 해당하면, checked 속성을 적용
            </td>
            <td>
                &lt;input type="checkbox" name="check-box" th:checked="true"&gt; checked <br>
                &lt;input type="checkbox" name="check-box" th:checked="false"&gt; unchecked <br>
                &lt;input type="checkbox" name="check-box" th:checked="true"&gt; checked <br>
                &lt;input type="radio" name="radio" th:checked="false"&gt; unchecked <br>
                &lt;input type="radio" name="radio" th:checked="true"&gt; checked <br>
            </td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>th:replace</td>
            <td>
                jsp의 replace와 유사한 속성 <br>
                th:fragment에 설정한 이름을 찾아 해당 코드로 치환 <br>
                경로를 작성할 때에는 ~{...}의 표현식으로 작성한다 <br>
                <strong>현재 태그를 fragment로 대체</strong>
            </td>
            <td>
                <div style="display: flex; flexflow: nowrap row;">
                    <code>
                        &lt;div th:replace="~{footer :: foot}"&gt; <br>
                        &emsp;<i>출력되지 않을 문자열</i> <br>
                        &lt;/div&gt;
                    </code>
                    &emsp;&emsp;&emsp;<br>
                    &emsp;→&emsp;
                    <code><br>
                        &lt;span th:fragment="foot"&gt;바닥글 조각&lt;/span&gt;
                    </code>
                </div>
            </td>
        </tr>
        <tr>
            <td>th:fragment</td>
            <td>
                th:replace에 적용될 조각. <br>
                <strong>반복할 컨텐츠에 이름을 설정하여 적용. </strong><br>
            </td>
            <td>
                <code>
                    &lt;span th:fragment="foot"&gt;바닥글 조각&lt;/span&gt;
                </code>
            </td>
        </tr>
        <tr>
            <td>th:insert</td>
            <td>
                th:fragment에 설정한 이름을 찾아 해당 코드를 삽입 <br>
                <strong>현재 태그의 컨텐츠에 fragment가 삽입</strong>
            </td>
            <td>
                <div style="display: flex; flexflow: nowrap row;">
                    <code>
                        &lt;div th:insert="~{footer :: foot}"&gt; <br>
                        &emsp;<i>출력되지 않을 문자열</i> <br>
                        &lt;/div&gt;
                    </code>
                    &emsp;&emsp;&emsp;<br>
                    &emsp;→&emsp;
                    <code>
                        &lt;div&gt; <br>
                        &emsp;&lt;span th:fragment="foot"&gt;바닥글 조각&lt;/span&gt; <br>
                        &lt;/div&gt;
                    </code>
                </div>
            </td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>th:attrprepend</td>
            <td>속성 앞에 추가</td>
            <td>
                <code>
                    &lt;div th:attrprepend="class='active '"&gt; <br>
                    &emsp;* 띄어쓰기를 직접 입력해서 구분 * <br>
                </code>
            </td>
        </tr>
        <tr>
            <td>th:attrappend</td>
            <td>속성 뒤에 추가</td>
            <td>
                <code>
                    &lt;div th:attrappend="class=' active'"&gt; <br>
                    &emsp;* 띄어쓰기를 직접 입력해서 구분 * <br>
                </code>
            </td>
        </tr>
        <tr>
            <td>th:classappend</td>
            <td>class 속성 뒤에 추가</td>
            <td>
                <code>
                    &lt;div th:classappend="'active'"&gt; <br>
                    &emsp;자동 공백 추가 <br>
                </code>
            </td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
</table>


### th:field <br>

![](/assets/image/2024-10-22-th_field_01.png) | ![](/assets/image/2024-10-22-th_field_02.png) 

>



<br>
<hr>
<br>


### th:each="${#numbers.sequence(start, end, step)}"

![](/assets/image/2024-10-27-thymeleaf-numbers.sequence.png)


### th:fragment <br>

![](/assets/image/2024-10-26-fragment_01.png) 
![](/assets/image/2024-10-26-fragment_03.png)


<br>
<hr>
<br>


## Thymeleaf-layout-dialect
Thymeleaf 내 코드의 재사용성을 높이기 위해서 <br>
레이아웃과 템플릿을 구축할 수 있는 오픈소스 라이브러리 <br>




## #Utility 

#message : 메시지, 국제화 처리 <br>
#uris : URI 이스케이프 지원 <br>
#dates : java.util.Date 서식 지원 <br>
#calendars : java.util.Calendar 서식 지원 <br>
#temporals : 자바8 날짜 서식 지원 (날짜는 주로 temporals를 많이 사용) <br>
#numbers : 숫자 서식 지원 <br>
#strings : 문자 관련 편의 기능 <br>
#objects : 객체 관련 기능 <br>
#bools : boolean 관련 기능 <br>
#arrays : 배열 관련 기능 <br>
#lists : 컬렉션 관련 기능 <br>
#sets : 컬렉션 관련 기능 <br>
#maps : 컬렉션 관련 기능 <br>
#ids : 아이디 처리 관련 기능 <br>

출처: [Steady and right:티스토리](https://maenco.tistory.com/entry/Thymeleaf-Utilities-유틸리티)


#numbers.formatInteger()

#numbers.sequence()

#temporals.format()

#dates.format()

#strings.substring()

#strings.length()


```html
<!-- 타임리프 유틸리티 객체와 대표 함수 예시 -->

<!-- 1. #message: 국제화 메시지 처리 -->
<p th:text="#{greeting.message}">Hello, world!</p>

<!-- 2. #uris: URI 조작 및 매개변수 추가 -->
<a th:href="@{${#uris.param('/search', 'query', 'thymeleaf')}}">Search</a>

<!-- 3. #dates 및 #temporals: 날짜 포맷팅 및 조작 -->
<p th:text="${#dates.format(today, 'yyyy-MM-dd')}"></p> <!-- #dates -->
<p th:text="${#temporals.format(today, 'yyyy/MM/dd')}"></p> <!-- #temporals -->

<!-- 4. #numbers: 숫자 포맷팅 -->
<p th:text="${#numbers.formatDecimal(price, 0, 'COMMA')}"></p>

<!-- 5. #strings: 문자열 조작 -->
<p th:if="${#strings.contains(name, 'thyme')}">Name contains 'thyme'</p>

<!-- 6. #objects: 객체 조작 -->
<p th:text="${#objects.nullSafe(user.name)}"></p>

<!-- 7. #bools: 불리언 값 조작 -->
<p th:if="${#bools.isTrue(isAvailable)}">Available</p>

<!-- 8. #arrays, #lists, #sets, #maps: 컬렉션 조작 -->
<ul th:each="item : ${#arrays.toList(items)}">
    <li th:text="${item}"></li>
</ul>

<!-- 9. #ids: 고유 ID 생성 -->
<div id="${#ids.next('prefix')}"></div>
```


<br>
<hr>

## Thymeleaf Layout Dialect

Thymeleaf는 서버 사이드 Java 템플릿 엔진으로,  <br>
JSP에 비해 HTML 친화적이지만,  <br>
템플릿에서 공통 레이아웃을 쉽게 재사용할 수 있는 방법이 부족했다.  <br>

기존 JSP와 달리 Thymeleaf는 HTML 구조를 최대한 유지하는 방향으로 설계되었고,  <br>
JavaScript 없이도 정적인 HTML 파일로 작동할 수 있도록 고안되었다.  <br>

그러다 보니 복잡한 웹 애플리케이션에서는 페이지마다  <br>
동일한 레이아웃을 중복 작성하거나 프레임워크 없이  <br>
수작업으로 레이아웃을 설정해야 하는 단점이 있었다.  <br>

이를 해결하고자 Thymeleaf에 **thymeleaf-layout-dialect**가 등장하게 되었다. <br>

<br>


### thymeleaf-layout-dialect 사용법

&lt;head&gt;, &lt;header&gt;, &lt;footer&gt;와 같은  <br>
공통 영역을 분리하여 하나의 템플릿 파일에서 관리<br>

**layout:decorate** <br>
**layout:fragment를 사용할 필요 없이 템플릿을 지정** <br>
**현재 HTML 템플릿이 다른 템플릿을 부모 레이아웃으로 사용**할 때 <br>
> ***자식 템플릿에서만 사용***

**부모 템플릿의 layout:fragment로 지정된 부분에 자식 템플릿의 내용이 삽입**. <br>


<pre class="rec bg-bk">
- 주요 체크리스트 <br>
1.	자식 템플릿에 layout:decorate를 지정하고, layout:fragment는 설정하지 않음. <br>
2.	부모 템플릿에 layout:fragment로 삽입될 위치 지정. <br>
3.	프래그먼트 파일에서 불필요한 &lt;html&gt;, &lt;head&gt;, &lt;body&gt; 태그를 제거. <br>
4.	파일 경로 및 th:replace 참조가 정확한지 확인. <br>
</pre>


<br>


- 의존성 설정 (gradle)
<pre><code>
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf' // Thymeleaf Template
	implementation 'nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect'       // Thymeleaf Layout
}
</code></pre>





<br>
<hr>
<br>

### ❌ 오류 : APPLICATION FAILED TO START
<details>
<summary class="summary-title">Web server failed to start. Port 80 was already in use.</summary>
<p>원인 : 포트 번호 중복</p>
<details>
<summary>방법 1> 이클립스 Boot Dashboard 확인</summary>
    <pre>
    1. 서버 종료 (콘솔창 확인)
    2. [Window] - [Show View] - 'Other' - 'Other' - 'Boot Dashboard'
    3. 'stop'
    4. 'restart'
    </pre>
</details>

<details>
<summary>방법 2> 프롬프트 포트 번호 삭제</summary>
    <pre>
    1. 포트 번호 확인 _ lsof -i:포트번호
    2. 포트 번호 삭제 _ kill -9 PID 
    .. 한번에 하려면 아래 (현재 port=80)
    %> kill -9 ${lsof -t -i:80}
    </pre>
</details>
</details>





--- 

참고 사이트 <br>
[https://adjh54.tistory.com/](https://adjh54.tistory.com/m/75) <br>
