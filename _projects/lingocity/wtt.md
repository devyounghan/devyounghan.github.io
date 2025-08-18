---
layout: page
parent: lingocity
title: 링고시티 Windows 툴
description: >
hide_description: true
accent_color: '#ffedb5'
accent_image:
  background: '#303244'
theme_color: '#193747'
sitemap: false
---

<div class="my-img-row">
  <figure>
    <img src="/assets/img/projects/lingocity/wtt/wtt.jpg" style="width:80%" oncontextmenu="return false;">
  </figure>
</div>

* 목차
{:toc}


### 개발 목적

기존의 복잡한 빌드 프로세스와 이로 인한 중간 산출물 확인의 불편함,<br>
모바일 기기와 PC를 오가며 작업해야 하는 비효율 등으로 인해 업무 생산성이 낮다고 느꼈습니다.<br>

이러한 문제를 해결하기 위해 접근성이 좋고 다양한 편의 기능을 갖춘 툴의 필요성을 인식하게 되었고,<br>
Widnows 환경에서 에디터와 유사한 작업 환경으로 개발자가 아닌 팀원들도 쉽게 개발 환경에 접근하고<br>
언제든 편리하게 작업할 수 있도록 하는 것을 목표로 툴을 개발하였습니다.<br>

이를 위해 기존 빌드 프로세스를 우회하고, 모바일 앱 실행 시 필요한 리소스와 데이터 다운로드 시스템을 별도로 처리했으며,<br>
툴에서만 사용할 수 있는 편의 기능을 개발하여 자주 반복하는 작업이나 양산 작업을 보다 효율적으로 진행할 수 있도록 하였습니다.<br>

Windows 툴은 팀원들이 필요로 하는 기능을 직접 요청 받고 추가하면서 개발 초기부터 꾸준히 업데이트해왔습니다.

<div style="margin-top:3rem;"></div>
<div class="my-img-row">
  <figure>
    <img src="/assets/img/projects/lingocity/wtt/wtt_login.jpg" style="width:80%" oncontextmenu="return false;">
    <figcaption>로그인 화면</figcaption>
  </figure>
</div>


### 주요 기능


#### 자유 시점 모드

툴에만 적용된 전용 모드입니다.<br>
카메라가 자유 시점으로 전환되어 지형에 상관 없이 자유롭게 이동할 수 있게 변경되고,<br>
기존 메인 UI 대신 개발용 UI가 등장하게 됩니다.<br>

개발용 UI에는 기획 단계에서 필요한 기능들을 추가하였습니다.<br>
에디터와 같이 오브젝트를 선택하거나 Transform을 변경할 수 있고,<br>
필요한 값을 직접 입력하여 데이터에 반영할 수 있게 하였습니다.<br>

이를 활용해 기획 단계에 필요한 오브젝트를 생성하고 기획자가 의도하는 대로 배치해<br>
화면 구성이나 구도를 직접 구상하고 수치 또한 구체적으로 확인할 수 있게 하였습니다.<br>

또한 씬에 존재하는 특정 오브젝트를 검색하고, 해당 위치로 이동할 수 있습니다.

<div style="margin-top:3rem;"></div>
<div class="my-img-row">
  <figure>
    <img src="/assets/img/projects/lingocity/wtt/wtt_free_1.jpg" style="width:80%" oncontextmenu="return false;">
    <figcaption>자유 시점 카메라</figcaption>
  </figure>
</div>
<div class="my-img-row">
  <figure>
    <img src="/assets/img/projects/lingocity/wtt/wtt_free_2.jpg" style="width:80%" oncontextmenu="return false;">
    <figcaption>NPC 생성 및 배치</figcaption>
  </figure>
</div>


#### 씬 이동

링고시티 내 다른 씬으로 바로 이동할 수 있는 기능입니다.

<div style="margin-top:3rem;"></div>
<div class="my-img-row">
  <figure>
    <img src="/assets/img/projects/lingocity/wtt/wtt_scene.gif" style="width:90%" oncontextmenu="return false;">
    <figcaption>씬 이동</figcaption>
  </figure>
</div>