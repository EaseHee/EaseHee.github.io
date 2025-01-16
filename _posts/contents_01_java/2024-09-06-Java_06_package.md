---
layout: single
title: "Java_06_접근 제한자와 패키지"
date: 2024-09-06
categories:
  - Java
tags:
  - Java
  - 접근 제한자
  - 패키지
---

<br>

---

<br>


### 접근 제한자
캡슐화된 클래스는 외부에서 접근할 수 있는 범위를 설정할 수 있는데, <br>
이 때 필요한 것이 **접근 제한자**이다. <br>

접근 제한자의 종류는 총 4가지로 다음과 같다. <br>

<style>
    tr th:Nth-of-type(1),td:Nth-of-type(1) {
        text-align: end
    }
    tr th:Nth-of-type(2),td:Nth-of-type(2) {
        text-align: end
    }
    tr th:Nth-of-type(3),td:Nth-of-type(3) {
        text-align: center
    }
    tr th:Nth-of-type(4),td:Nth-of-type(4) {
        text-align: center
    }
</style>

<table  style="width: fit-content">
    <thead>
        <tr>
            <th>접근 제한자</th>
            <th>제한 범위</th>
            <th>동일 패키지 접근 여부 <br>(상속받지 않은 경우)</th>
            <th>상속 클래스 접근 여부 <br>(패키지가 다른 경우)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i><b>public</b></i></td>
            <td>모든 범위의 클래스</td>
            <td>O</td>
            <td>O</td>
        </tr>
        <tr>
            <td><i><b>default</b></i></td>
            <td>동일 패키지 내 클래스</td>
            <td>O</td>
            <td>X</td>
        </tr>
        <tr>
            <td><i><b>protected</b></i></td>
            <td>상속받은 클래스</td>
            <td>X</td>
            <td>O</td>
        </tr>
        <tr>
            <td><i><b>private</b></i></td>
            <td>외부의 모든 접근 제한</td>
            <td>X</td>
            <td>X</td>
        </tr>
    </tbody>
</table>
<br>
***접근 제한자를 명시적으로 작성하지 않을 경우 default로 설정된다.*** <br>
<br>



### 패키지
패키지는 작업 폴더의 개념으로 보면 동일한 폴더에 위치한 클래스를 의미하는데 <br>
클래스의 소속을 정의해준다. <br>
<br>

**java.lang.Object** <br>
자바에서 가장 기본이 되는 클래스인 Object는 <br>
java라는 패키지 내 lang 패키지 소속 클래스이다. <br>
이렇게 패키지는 상위 폴더부터 경로를 차례로 입력해주는데 <br>
프로젝트 단위의 경우 관례적으로 회사의 domain 주소를 거꾸로 작성한다고 한다. <br>
> ex) com.happy.Clazz <br>
Clazz 클래스를 com 폴더 내 happy 폴더에 작성 <br>
(관례 패키지명은 소문자로 작성)
<br>


<br><br>
<div class="image-container" style="border: 2px solid white;">
    <img class="image-medium" src="/assets/image/2024-08-09-Java-Package-01.png">
</div>
> 패키지가 다른 경우는 public으로 설정해야 접근 가능하다

<br>
<div class="image-container" style="border: 2px solid white;">
    <img class="image-medium" src="/assets/image/2024-08-09-Java-Package-02.png">
</div>
> 폴더가 없더라도 jar가 있다면 접근할 수 있다. <br>
jar파일에 CLASSPATH를 설정하고 java를 실행. <br>

<br>
클래스의 유효 범위에 대한 또다른 이야기는 [라이브러리와 프레임워크](/java/Java_07_Library)에서 짧게 해보려고 한다.