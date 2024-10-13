---
layout: post
title: "Semantic Tag"
date: 0000-01-01
categories: blog
---

<br>

---
### semantic tag
시맨틱(semantic)이란 '의미의, 의미론적인'이라는 뜻. <br>
태그 내 컨텐츠의 의미를 담고 있는 태그. <br>
웹 문서의 구조와 의미를 명확히 표현하기 위해 사용. <br>

HTML5 이후에 사용. <br>
HTML4 이전에는 &lt;div&gt;나 &lt;span&gt; 등이 사용. <br>

<br>
<div class="image-container">
    <img class="image-medium" src="/assets/image/SemanticTag.png">
</div>
> 이미지 출처 : [https://www.semrush.com](https://www.semrush.com/blog/semantic-html5-guide/)

<br>

- Semantic Tag 용레
<pre><code>
&lt;main&gt;
    &lt;h2&gt;이 페이지의 주요 콘텐츠&lt;/h2&gt;
    &lt;p&gt;여기에 페이지의 핵심 내용이 들어갑니다.&lt;/p&gt;
&lt;/main&gt;

&lt;header&gt;
    &lt;h1&gt;웹사이트 이름&lt;/h1&gt;
    &lt;nav&gt;
        &lt;ul&gt;
            &lt;li&gt;&lt;a href="#">홈&lt;/a&gt;&lt;/li&gt;
            &lt;li&gt;&lt;a href="#">소개&lt;/a&gt;&lt;/li&gt;
            &lt;li&gt;&lt;a href="#">연락처&lt;/a&gt;&lt;/li&gt;
        &lt;/ul&gt;
    &lt;/nav&gt;
&lt;/header&gt;

&lt;footer&gt;
    &lt;p&gt;&copy; 2024 회사명. All rights reserved.&lt;/p&gt;
    &lt;nav&gt;
        &lt;ul&gt;
            &lt;li&gt;&lt;a href="#">이용 약관&lt;/a&gt;&lt;/li&gt;
            &lt;li&gt;&lt;a href="#">개인정보 보호 정책&lt;/a&gt;&lt;/li&gt;
        &lt;/ul&gt;
    &lt;/nav&gt;
&lt;/footer&gt;

&lt;nav&gt;
    &lt;ul&gt;
        &lt;li&gt;&lt;a&gt;href="#">홈&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a&gt;href="#">블로그&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a&gt;href="#">연락처&lt;/a&gt;&lt;/li&gt;
    &lt;/ul&gt;
&lt;/nav&gt;

&lt;article&gt;
    &lt;h2&gt;블로그 포스트 제목&lt;/h2&gt;
    &lt;p&gt;이 글은 블로그 포스트의 내용입니다.&lt;/p&gt;
&lt;/article&gt;

&lt;section&gt;
    &lt;h2&gt;주제 1&lt;/h2&gt;
    &lt;p&gt;이 섹션에서는 주제 1에 대해 설명합니다.&lt;/p&gt;
&lt;/section&gt;

&lt;aside&gt;
    &lt;h3&gt;관련 글&lt;/h3&gt;
    &lt;ul&gt;
        &lt;li&gt;&lt;a&gt;href="#">관련 링크 1&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a&gt;href="#">관련 링크 2&lt;/a&gt;&lt;/li&gt;
    &lt;/ul&gt;
&lt;/aside&gt;

&lt;figure&gt;
    &lt;img src="image.jpg" alt="설명 이미지"&gt;
    &lt;figcaption&gt;이 이미지는 설명을 제공합니다.&lt;/figcaption&gt;
</figure>

&lt;figure&gt;
    &lt;img src="image.jpg" alt="설명 이미지"&gt;
    &lt;figcaption&gt;이 이미지는 설명을 제공합니다.&lt;/figcaption&gt;
&lt;/figure&gt;
</code></pre>

<style>
    th, td {
        text-align: center;
    }
</style>

<table>
    <thead>
        <tr>
            <th></th><th></th>
        </tr>
        <tr>
            <th>태그</th>
            <th>특징</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>&lt;main&gt;</td>
            <td>현재 문서의 주된 콘텐츠를 작성하는 영역</td>
        </tr>
        <tr>
            <td>&lt;header&gt;</td>
            <td>문서의 제목, 머리말 영역</td>
        </tr>
        <tr>
            <td>&lt;footer&gt;</td>
            <td>문서의 하단, 꼬리말, 웹페이지내의 회사정보, 저작권 정보, 연락처등을 제공</td>
        </tr>
        <tr>
            <td>&lt;nav&gt;</td>
            <td>웹페이지의 나침판 역할로 다른페이지, 사이트 이동의 링크 작성 영역<br>&lt;a&gt; 태그들이 나열</td>
        </tr>
        <tr>
            <td>&lt;aside&gt;</td>
            <td>사이드바, 광고, 배너 등을 표시하는 양쪽 영역</td>
        </tr>
        <tr>
            <td>&lt;section&gt;</td>
            <td>관련된 콘텐츠들을 묶어 구분하는 영역</td>
        </tr>
        <tr>
            <td>&lt;article&gt;</td>
            <td>본문과 독립된 콘텐츠를 작성하는 영역</td>
        </tr>
        <tr>
            <td>&lt;figure&gt;</td>
            <td>이미지나 그래픽과 같은 시각적 콘텐츠를 묶는 영역</td>
        </tr>
        <tr>
            <td>&lt;figcaption&gt;</td>
            <td>figure 안에 포함된 콘텐츠에 대한 설명을 제공하는 영역</td>
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

[참고 사이트]<br>
[https://sharonprogress.tistory.com](https://sharonprogress.tistory.com/104) <br>
[https://www.semrush.com](https://www.semrush.com/blog/semantic-html5-guide/) <br>