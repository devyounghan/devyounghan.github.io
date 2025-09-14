---
layout: page
parent: lingocity
title: 버디(펫)
description: >
hide_description: true
accent_color: '#ffedb5'
accent_image:
  background: '#303244'
theme_color: '#193747'
sitemap: false
---

<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_intro_anim.gif" style="width:50%" oncontextmenu="return false;">
  </figure>
</div>

* 목차
{:toc}


### 소개

버디는 링고시티에서의 펫을 의미합니다.<br>
단순한 동행을 넘어 유저와 상호작용하고 다양한 즐거움을 주는 링고시티의 주요 콘텐츠입니다.


### 성장 시스템

버디는 성장 시스템을 가지고 있습니다.<br>
각 버디에는 레벨이 있으며 특정 레벨에 도달할 때마다 단계가 상승하여 진화하고,<br>
이에 맞게 외형과 애니메이션이 변화합니다.<br>

버디는 먹이를 주거나 특정 명령어를 음성으로 입력해 상호작용하는 방식으로 성장시킬 수 있습니다.<br>
이러한 성장 상태(경험치, 레벨, 단계)는 변화가 생길 때마다 서버에 저장됩니다.

<div style="margin-top:2rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_1.png" oncontextmenu="return false;">
    <figcaption>1단계 레서팬더</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_2.png" oncontextmenu="return false;">
    <figcaption>2단계 레서팬더</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_3.png" oncontextmenu="return false;">
    <figcaption>3단계 레서팬더</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_4.png" oncontextmenu="return false;">
    <figcaption>4단계 레서팬더</figcaption>
  </figure>
</div>


### 명령어 음성 인식

유저는 버디에게 특정 명령어를 음성으로 입력해 버디와 상호작용할 수 있습니다.<br>
명령어를 올바르게 발화하여 성공하면 버디는 해당 명령에 맞는 행동을 취하고 경험치를 획득합니다.<br>
버디의 레벨이 오를수록 할 수 있는 행동이 늘어나게 됩니다.

발화를 중심으로 하는 링고시티 콘텐츠 컨셉에 맞추어<br>
유저의 발화를 통해 버디와 상호작용하는 것을 목표로 해당 기능을 개발하였습니다.

<div style="margin-top:2rem;"></div>
<div class="my-media-row">
 <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_training_anim.gif" style="width:80%" oncontextmenu="return false;">
    <figcaption>명령어 음성 인식</figcaption>
  </figure>
</div>


### FSM

버디는 FSM을 적용하여 각 상태를 기반으로 행동하게끔 개발하였습니다.<br>
버디가 가질 수 있는 상태를 구분하였고, 개발 시 버디의 상태 추가, 수정이 용이하도록 설계하고 개발하였습니다.


### 신규 버디 추가

버디는 유저들이 가장 좋아하는 콘텐츠 중 하나였고, 이에 맞추어 신규 버디를 추가하기로 결정하었습니다.<br>
버디 획득, 교체 등의 신규 기능 개발과 함께 이후의 확장성을 고려하여 기존의 관련 코드와 구조를 전면 개편하였습니다.

<div style="margin-top:2rem;"></div>
<div class="my-media-row">
 <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_dragon_new.jpg" style="width:70%" oncontextmenu="return false;">
    <figcaption>신규 버디 출시 기념 배너</figcaption>
  </figure>
</div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_dragon_acquisition_anim.gif" style="width:80%" oncontextmenu="return false;">
    <figcaption>신규 버디 획득 연출</figcaption>
  </figure>
</div>


### 아바타

신규 버디 출시에 맞추어 버디 전용 아바타 기능도 함께 개발하였습니다.<br>
기존 캐릭터 아바타와는 다르게 버디만의 특수성으로 인해<br>
버디 아바타 시스템에만 별도로 적용할 새로운 정책들과 기능이 필요했고,<br>
개발 과정에서 많은 의논과 테스트를 거쳐 기능 개발을 완료하였습니다.

<div style="margin-top:2rem;"></div>
<div class="my-media-row">
 <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_avatar.jpg" style="width:70%" oncontextmenu="return false;">
    <figcaption>버디 아바타 보관함</figcaption>
  </figure>
</div>