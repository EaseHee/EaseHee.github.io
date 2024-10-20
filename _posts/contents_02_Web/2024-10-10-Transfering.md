---
layout: post
title: "sendRedirect 와 forward"
date: 2024-10-10
categories: blog
category: Web
---

<br>

---
### **클라이언트측 이동 방식** vs **서버측 이동 방식** <br>
두 이동 방식은 크게 프로젝트 내 ***WEB-INF 폴더 접근 여부***와 <br>
웹 페이지 주소창의 ***주소 노출 여부***에서 차이가 있다. <br>
<br>
**클라이언트측 이동**은 주소창에 페이지의 주소가 노출된다. <br>
또한 프로젝트 내 WEB-INF 폴더로의 접근이 제한된다. <br>
sendRedirect는 이런 클라이언트측 이동 방식이다. <br>
<br>
반면, **서버측 이동**은 주소창에 페이지의 주소가 노출되지 않는다. <br>
따라서 사용자는 현재 위치한 페이지의 주소를 알 수 없다. <br>
뿐만 아니라 서버 측에서는 WEB-INF 폴더 내의 접근 또한 자유로우며 <br>
Servlet 객체를 활용하여 페이지를 이동시키는 경우에 forward 방식을 사용할 수 있다. <br>

<table>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
        <tr>
            <th></th>
            <th>sendRedirect</th>
            <th>forward</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>이동 주체</td>
            <td>클라이언트측 이동</td>
            <td>서버측 이동</td>
        </tr>
        <tr>
            <td>WEB-INF 폴더 접근 여부</td>
            <td>X</td>
            <td>O</td>
        </tr>
        <tr>
            <td>주소 노출 여부</td>
            <td>O</td>
            <td>X</td>
        </tr>
        <tr>
            <td>URL 변경 여부</td>
            <td>O : 새로운 URL 요청</td>
            <td>X : 요청된 URL 유지</td>
        </tr>
        <tr>
            <td>데이터 유지 여부</td>
            <td>X : 새로운 요청 생청 <br> 쿠키, 세션, URL 변수와 같은 메커니즘 사용</td>
            <td>O : 요청과 응답 객체 공유 <br> 서블릿, JSP 페이지 간 데이터 전달 용이</td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
</table>
