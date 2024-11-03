---
layout: post
title: "UML_ClassDiagram"
date: 2024-11-01
categories: blog
category: Diagram
---

<br>

---
<br>

## 클래스 다이어그램 <br>
클래스간 관계를 이해하기 쉽도록 그림(다이어그램)으로 표현한 것.

<br>

### 관계 구조


<pre class="rec padding">
<span>참조 관계 유지에 따른 구분</span> '멤버변수 | 지역변수'
    - 연관 관계 (association)<span> : 참조 관계 유지</span> '멤버변수'
        <span>-> 집합 관계 여부에 따른 구분</span>
        - 일반 연관 '메서드 호출 시'
        - 특수 연관<span> : 집합 관계</span> '생성자 호출 시'
            <span>-> LifeCyle에 따른 구분</span> '인스턴스 생성, 소멸 시점의 차이'
            - 집합 연관 (Aggregation)<span> : 생성된 인스턴스를 주입받는 경우</span>
            - 합성 연관 (Composition)<span> : 생성자를 직접 호출하는 경우</span>
    - 의존 관계 (dependency)<span> : 참조 관계 소멸</span> '지역변수'
</pre>    
<pre>       <span>(의존 관계 세부 구분 시) 
            &lt;&lt;stereotype&gt;&gt;으로 작성 "UseCase에서 주로 사용"</span>
        <span>-> 기능의 포함, 확장에 따른 구분</span>
        - 포함 의존 (Include)  <span><b>"기능을 항상 포함"</b></span>
            <span> : 다른 유즈케이스의 기능을 포함하는 경우 (동작 수행)</span>
        - 확장 의존 (Extend)   <span>"기능의 조건부 확장"</span>
            <span> : 다른 유즈케이스의 기능을 추가할 수 있는 경우</span>
</pre>


<br>

### 표기법 

<table class="rec">
    <tr>
        <td>접근제한자</td><td>(+)public<br>(-)private<br>(#)protected<br>(~)default</td>
    </tr>
    <tr>
        <td>키워드</td><td>static(밑줄)<br>final{frozen} or {readonly}</td>
    </tr>
    <tr>
        <td>생성자</td><td>&lt;&lt;create&gt;&gt;</td>
    </tr>
    <tr>
        <td>메서드</td><td>변수명, 변수타입, 메서드명, 인자, 리턴타입 작성</td>
    </tr>
</table>


<br>

### 구성요소

<table class="rec">
    <thead>
        <tr>
            <th>대분류</th><th>소분류</th><th>표기법</th><th>예시</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="4">연관 관계<br>(Association)</td>
            <td>
                일반 연관 관계<br>
                (Association)
            </td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.002.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code>
public class A {
    private List&lt;B&gt; fieldA; // 제네릭 타입 등 간접적으로 연관
    ...
}
                </code></pre>
            </td>
        </tr>
        <tr>
            <td>
                직접 연관 관계<br>
                (Direct-Association)
            </td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.003.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code type="text/java">
public class A {
    public B fieldA; // B 클래스를 직접 참조하는 경우

    public void methodA() {
        fieldA.methodB(); // B 클래스의 기능 수행
    }
}
                </code></pre>
            </td>
        </tr>
        <tr>
            <td>
                집합 연관 관계<br>
                (Aggregation)
            </td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.004.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code type="text/java">
public class A {
    public B fieldA;

    public A(B b) {
        fieldA = b; // A 생성자 호출 이전에 B 인스턴스 생성 후 주입
                    // A가 소멸한 이후에도 B 인스턴스는 유지된다.
    }
}
                </code></pre>
            </td>
        </tr>
        <tr>
            <td>
                합성 연관 관계<br>
                (Composition)
            </td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.005.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code type="text/java">
public class A {
    public B fieldA;

    public A() {
        fieldA = new B(); // A 생성자에서 B 생성자 호출
                          // A가 소멸하게 되면 B도 함께 소멸된다.
    }
}
                </code></pre>
            </td>
        </tr>
    </tbody>
    <tbody>
        <tr>
            <td colspan="2">의존 관계<br>(Dependency)</td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.001.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code type="text/java">
public class A {
    public method1(B b) { // 지역변수, 매개변수 
        ...
    }

    public B method2() { // 반환 타입
        B b = new B();
        return b; 
    }
}
                </code></pre>
            </td>
        </tr>
    </tbody>
    <tbody>
        <tr>
            <td colspan="2">일반화 관계<br>(Generalization)</td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.006.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code type="text/java">
public class A extends B { // 클래스 B를 상속
    public void methodA() {
        fieldB = ""; // B 클래스의 인스턴스 생성없이 멤버 변수,
        methodB();   // 메서드에 바로 접근할 수 있다.
    }
}

public class B {
    public String fieldB;

    public void methodB() {
        ...
    }
}
                </code></pre>
            </td>
        </tr>
        <tr>
            <td colspan="2">실체화 관계<br>(Realization)</td>
            <td>
                <image src="/assets/image/2024-11-02-UML_ClassDiagram.007.png" class="fix-size-sm"/>
            </td>
            <td>
                <pre><code type="text/java">
public class A implements B { // 인터페이스 B를 구현
    @Override
    public void methodB() { 
        String fieldA = FIELD_B;
        ...
    }
}

public interface B {
    String FIELD_B; // public static final

    void methodB(); // public abstract 
}
                </code></pre>
            </td>
        </tr>
    </tbody>

</table>

<br>



---

[이미지 출처 | 참고 사이트] <br> 
[https://jerocaller.github.io](https://jerocaller.github.io/tip/how-to-draw-io/) <br>
[https://earthkingman.tistory.com](https://earthkingman.tistory.com/118) <br>
[https://itproda.tistory.com](https://itproda.tistory.com/101) <br>

<style>
    span {
        color: gray;
        font-size: 1rem;
    }
    .rec {
        border: solid 1.5px;
        border-radius: 20px;
    }
    .fix-size-sm {
        min-width: 400px;
    }
    table {
        text-align: center;
        padding: 5%;
    }
    th {
        text-align: center;
    }
    code {
        text-align: left;
    }
</style>