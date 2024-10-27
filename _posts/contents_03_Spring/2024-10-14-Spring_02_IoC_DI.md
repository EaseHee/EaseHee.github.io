---
layout: post
title: "Spring_02_제어 역전과 의존성 주입"
date: 2024-10-14
categories: blog
category: Spring
---

<br>

---

### 제어 역전 (IoC : Inversion of Control)
메서드나 객체의 호출이 개발자에 의해 결정되는 것이 아니라, 외부에서 결정되는 것을 의미. <br>

> 즉, 개발자가 제어의 흐름을 직접 통제하는 기존의 방식이 아니라 <br>
외부의 프레임워크나 라이브러리가 제어의 흐름을 대신 통제하는 것. <br>


![](/assets/image/2024-10-13-Framework_Library.png) | ![](/assets/image/spacer.png)

> IoC (제어 역전) 모식도 <br>

<br>

<table>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
    </thead>
    <thead>
        <tr>
            <th colspan="2">제어의 역전에 따른 장단점</th>
            <!-- <th></th> -->
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">장점</td>
            <td>코드의 유연성 및 재사용성 증가</td>
        </tr>
        <tr>
            <!-- <td></td> -->
            <td>객체간 의존성 및 결합도 저하</td>
        </tr>
        <tr>
            <!-- <td></td> -->
            <td>IoC 컨테이너 사용 시 의존성 관리가 용이<br>(코드 변경 용이)</td>
        </tr>
        <tr>
            <td rowspan="3">단점</td>
            <td>코드의 복잡성 증가<br>(가독성 저하)</td>
        </tr>
        <tr>
            <!-- <td></td> -->
            <td>IoC 컨테이너의 복잡도에 따른<br>초기 설정, 성능 이슈 발생 우려</td>
        </tr>
        <tr>
            <!-- <td></td> -->
            <td>생성된 객체에 대한 이해 부족 우려 </td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
    </thead>
</table>

<br>
<hr>
<br>



### 의존성 주입 (DI : Dependency Injection)
의존성 주입이란 외부 컨테이너가 생성한 객체를 주입받아 사용하는 방식. <br>
결합도를 낮추고 유연성을 확보해준다. <br>
의존성 주입 방식은 세 가지가 있다. <br>
- 생성자 주입<br>
- setter 메서드 주입<br>
- 필드 객체 선언 주입<br>
<br>
<hr>
<br>


> xml 파일 예제
<pre><code>
&lt;!-- 1&gt; 핵심 로직 : AOP 입장에서는 "target" --&gt;
&lt;bean id="targetClass" class="pack.MessageImpl"&gt;
    &lt;property name="name" value="한국인"/&gt;
&lt;/bean&gt;
&lt;!-- 2&gt; Advice(Aspect)를 Target에 weaving (Advice : 실질적으로 동작할 관심사항 코드) --&gt;
&lt;bean id="loggingAdvice" class="advice.LoggingAdvice"/&gt;
&lt;!-- 원리를 이해할 것 --&gt;

&lt;!-- 3&gt; Proxy를 통한 간접 접근 (Proxy : 캐시) | ProxyFactoryBean : 프록시 인스턴스 생성 --&gt;
&lt;bean id="proxy" class="org.springframework.aop.framework.ProxyFactoryBean"&gt;
    &lt;property name="target"&gt;
        &lt;ref bean="targetClass"/&gt;
    &lt;/property&gt;
    &lt;!-- 인터셉터명 list로 작성 --&gt;
    &lt;property name="interceptorNames"&gt;
        &lt;list&gt;
            &lt;value&gt;hiAdvisor&lt;/value&gt;
        &lt;/list&gt;
    &lt;/property&gt;
&lt;/bean&gt;

&lt;!-- 4&gt; Advisor (Advice + Pointcut) | DefaultPointcutAdvisor : advice와 pointcut을 연결해주는 객체 --&gt;
&lt;bean id="hiAdvisor" class="org.springframework.aop.support.DefaultPointcutAdvisor"&gt;
&lt;!-- setter injection --&gt;
    &lt;!-- Advice --&gt;
    &lt;property name="advice"&gt;
        &lt;ref bean="loggingAdvice"/&gt;
    &lt;/property&gt;
    &lt;!-- Pointcut --&gt;
    &lt;property name="pointcut"&gt;
        &lt;bean class="org.springframework.aop.support.JdkRegexpMethodPointcut"&gt;
            &lt;!-- 정규 표현식을 사용 --&gt;
            &lt;property name="pattern"&gt;
                &lt;value&gt;.*sayHi*.&lt;/value&gt;&lt;!-- 현재 패키지 내에 sayHi라는 문자열을 포함한 메서드를 setPattern(메서드)에 전달 --&gt;
            &lt;/property&gt;
        &lt;/bean&gt;
    &lt;/property&gt;
&lt;/bean&gt;
</code></pre>



> 생성자와 setter 메서드를 풀어써본 것
<pre><code>
    1> target : 핵심로직
    &lt;bean id="targetClass" class="pack.MessageImpl">
    ...
        
        MessageImpl targetClass = new MessageImpl();

        public MessageImpl() {
            setName("한국인");
        }


    2> Advice : target에 삽입될 aspect 코드
    &lt;bean id="loggingAdvice" class="advice.LoggingAdvice"/>

        LoggingAdvice loggingAdvice = new LoggingAdvice();


    3> Proxy : 캐시 ...
    &lt;bean id="proxy" class="org.springframework.aop.framework.ProxyFactoryBean">
    ...

        Proxy proxy = ProxyFactoryBean.getProxy();
    
        public Proxy getProxy() {
            setTarget(targetClass);
            setInterceptorNames("hiAdvisor");
        }

        public void setTarget (TargetClass targetClass) { // org.springframework.aop.framework.AdivosedSupport
            ...
        }

        public void setInterceptorNames (String... interceptorNames) {
            this.interceptorNames = interceptorNames;
        }
    
    4> Advisor : advice와 target을 연결
    &lt;bean id="hiAdvisor" class="org.springframework.aop.support.DefaultPointcutAdvisor">
    ...
        
        HiAdvisor hiAdvisor = new HiAdvisor()

        public HiAdvisor() {
            // AOP가 호출
            setAdvice(loggingAdvice);
            setPointCut(jdkRegexpMethodPointcut);
        }

        public void setAdvice(LoggingAdvice loggingAdvice) {
            this.loggingAdvice = loggingAdvice;
        }

        public void setPointCut(JdkRegexpMethodPointcut jdkRegexpMethodPointcut) {
            this.jdkRegexpMethodPointcut = jdkRegexpMethodPointcut;
        }

        public JdkRegexpMethodPointcut () {
            setPattern(".*sayHi*."); // AOP가 호출
        }
        public void setPattern (Regexp regexp) {
            ...
        }
</code></pre>

