---
layout: single
title: "Java_07_라이브러리와 프레임워크"
date: 2024-09-07
categories:
  - Java
tags:
  - Java
  - 라이브러리
  - 프레임워크
---

<br>

---

<br>

### 라이브러리


- jar와 library

자바의 클래스는 패키지로 구분하여 접근 범위에 제한 "***default***"를 둘 수 있고, <br>
상속을 통해 예외 "***protected***"를 둘 수도 있다. <br>

하지만 이건 어디까지나 동일한 프로젝트 내의 클래스에 해당하는 이야기이다. <br>
만약 다른 프로젝트에서도 해당 클래스의 기능을 사용하고 싶다면 어떻게 해야할까? <br>

이 때 사용하는 것이 바로 **jar** 압축파일이다. 
<br><br>

<div class="image-container" style="border: 2px solid white;">
    <img class="image-medium" src="/assets/image/2024-08-09-Java-Package-03.png">
</div>
> jar로 압축하는 방법

<br>

jar는 클래스의 소스코드를 컴파일한 **바이트코드들을 압축한 파일**이다. <br>
패키지의 정보도 포함하고 있기 때문에 ***import***하여 접근할 수 있다. <br>
> import org.json.JSONObject; <br>
import lombok.Data;

이렇게 다른 프로젝트의 클래스에 접근할 수 있도록 한 것을 일컬어 **library**라고 한다. <br>
> json 라이브러리, lombok 라이브러리, ...

하나의 프로젝트에서 여러 라이브러리를 사용할 수 있고, <br>
하나의 클래스에도 여러 라이브러리를 import하여 사용할 수 있다. <br>

이렇게 클래스로 기능을 설계를 하는 과정에서 ***필요한 재료를 추가***할 수 있는 것이 **library의 장점**이라고 할 수 있다. <br>

<br>
<hr>
<br>

### 프레임워크

framework는 클래스와 라이브러리의 집합체와 같다. <br>
개발에 필요한 많은 기능을 하나의 framework에서 한번에 제공하는 방식이다. <br>

이 경우 framework 하나를 사용하기만 하면 개발에 필요한 여러 소스를 제공받을 수 있기 때문에 <br>
라이브러리를 일일이 추가하고 불러올 필요가 없다. <br>
하지만 framework는 이미 만들어진 틀이 있어 개발자가 해당 틀(설정)에 맞게 코드를 작성해야 한다. <br>

<style>
    table {
        width: 35rem;
    }
    th, td {
        text-align: center
    }
</style>

<table>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
    </thead>
    <thead>
        <tr>
            <th colspan="2">framewok 사용에 따른 장단점</th>
            <!-- <th></th> -->
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">장점</td>
            <td>개발 시간 단축</td>
        </tr>
        <tr>
            <!-- <td></td> -->
            <td>일정 수준 이상 기대</td>
        </tr>
        <tr>
            <!-- <td>X</td> -->
            <td>유지 보수 용이</td>
        </tr>
        <tr>
            <td rowspan="3">단점</td>
            <td>습득 시간이 오래 걸림</td>
        </tr>
        <tr>
            <!-- <td>X</td> -->
            <td>개발의 자유도 제한</td>
        </tr>
        <tr>
            <!-- <td>X</td> -->
            <td>개발자의 능력 저하</td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
    </thead>
</table>

<br>
프레임워크에는 여러 종류가 있지만 대표적으로는 아래와 같이 구분할 수 있다.

<table>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
    </thead>
    <thead>
        <tr>
            <th>구분</th>
            <th>종류</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>자바 프레임워크</td>
            <td>Spring, 전자정부 프레임워크, Struts, ...</td>
        </tr>
        <tr>
            <td>ORM 프레임워크</td>
            <td>MyBatis(iBatis), Hibernate, ...</td>
        </tr>
        <tr>
            <td>자바스크립트 프레임워크</td>
            <td>React, AngularJS, Polymer, Ember, ...</td>
        </tr>
        <tr>
            <td>프론트엔드 프레임워크</td>
            <td>Bootstrap, Foundation, MDL, ...</td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
    </thead>
</table>

<br>


library를 사용할 때에는 개발자가 코드를 제어하는 방식이었다면 <br>
framework를 사용함으로써 제어권이 개발자에게서 framework로 넘어갔다고 볼 수 있다. <br>
이걸 **제어 역전(IoC : Inversion of Control)**이라고 한다. <br>


<br>
<div class="image-container">
    <img class="image-medium" src="/assets/image/2024-10-13-Framework_Library.png">
</div>
> IoC (제어 역전) 모식도 <br>

<br>
<hr>
<br>

### 제어 역전 (IoC : Inversion of Control)
메서드나 객체의 호출이 개발자에 의해 결정되는 것이 아니라, 외부에서 결정되는 것을 의미한다. <br>

> 즉, 개발자가 제어의 흐름을 직접 통제하는 기존의 방식이 아니라 <br>
외부의 프레임워크나 라이브러리가 제어의 흐름을 대신 통제하는 것을 말한다. <br>


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

[참고 사이트]<br>
[https://velog.io/@kwontae1313](https://velog.io/@kwontae1313/%EC%A0%9C%EC%96%B4-%EC%97%AD%EC%A0%84IoC%EA%B3%BC-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85DI) <br>
[https://velog.io/@whitecloud94](https://velog.io/@whitecloud94/%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-vs-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC) <br>
[https://sharonprogress.tistory.com](https://sharonprogress.tistory.com/169) <br>
[http://tobetong.com](http://tobetong.com/?p=6640) <br>