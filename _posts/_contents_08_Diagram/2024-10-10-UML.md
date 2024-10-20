---
layout: post
title: "UML_Eclipse 설치 [Mac]"
date: 2024-10-10
categories: blog
category: Diagram
---

<br>

---
<br>

UML : Unified Modeling Language <br>
시스템이나 프로세스를 이해하기 쉽도록 다이어그램으로 표현한 것.


## [Amateras UML](https://ko.osdn.net/projects/amateras/releases)
이클립스에 설치하기 (Eclipse) <br>

<br>

**1. [GEF](http://download.eclipse.org/tools/gef/updates/releases/) 설치 (Graphical Editing Framework)** <br>

Eclipse: [help] >  [install software] > work with: http://download.eclipse.org/tools/gef/updates/releases/

<div class="text-center">
    <img src="/assets/image/Install_GEF_Eclipse.png" class="image-responsive"/>
    <span>
        GEF Plugin 설치 (Eclipse)<br>
    </span>
</div>


<br>

**2. [ERD / UML](http://takezoe.github.io/amateras-update-site) 설치 (Entity Relational Diagram / Unified Modeling Language)** <br>

Eclipse: [help] >  [install software] > work with: http://takezoe.github.io/amateras-update-site
<br>
Eclipse Restart > [New] > [Others] > Amateras
<br>

<div class="text-center">
    <img src="/assets/image/Install_UML_Eclipse_Folder01.png" class="image-responsive"/>
    <img src="/assets/image/Install_UML_Eclipse_Folder02.png" class="image-responsive"/>
    <span>
        Amateras UML 플러그인 Eclipse 실행 패키지 폴더 내 dropins에 복사<br>
    </span>
</div>
<br>

[플러그인 파일] 
<br> 1. 
[net.java.amateras.umleditor_1.3.4.jar](/assets/image/net.java.amateras.umleditor_1.3.4.jar)
<br> 2. 
[net.java.amateras.umleditor.java_1.3.4.jar](/assets/image/net.java.amateras.umleditor.java_1.3.4.jar)
<br> 3. 
[net.java.amateras.xstream_1.3.4](/assets/image/net.java.amateras.xstream_1.3.4)

<br>


**3. 실행**
<br> 1. Eclipse: [Window] > [Show View] > [Others]
<br> 2. [Amateras UML] > [Stack Trace Sample]
<br> 3. 화면 우측 [Stack Trace Sample] 탭 > ['i' 모양 아이콘 : generate 버튼] 선택 > 프로젝트 / 패키지를 선택



<br>


---

[이미지 출처 | 참고 사이트] <br> 
[maykim51.tistory.com](https://maykim51.tistory.com/entry/%EC%84%A4%EC%B9%98%EB%B0%A9%EB%B2%95-Amateras-UML-%EB%A7%A5%EB%B6%81-Eclipse%EC%97%90-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)