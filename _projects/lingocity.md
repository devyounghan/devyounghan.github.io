---
layout: project
title: 링고시티
caption: 메타버스, 생성형 AI, 영어 회화
description: >
date: 2 Oct 2024
image: 
  path: /assets/img/projects/lingocity.png
  # srcset: 
  #   1920w: /assets/img/projects/lingocity.png
  #   960w:  /assets/img/projects/lingocity@0,5x.png
  #   480w:  /assets/img/projects/lingocity@0,25x.png
links:
  - title: 제품 소개 페이지
    url: https://m.wjthinkbig.com/prod/subjectDetail.do?subjectId=S0000087
accent_color: '#ffedb5'
accent_image:
  background: '#303244'
theme_color: '#193747'
sitemap: false
---

* 목차
{:toc}


## 소개
{:.my-heading-circle-1}

---

링고시티는 2024년 10월 출시된 메타버스 기반 영어 회화 학습 제품입니다.<br>
메타버스로 구현된 전 세계 주요 국가를 배경으로 다양한 콘텐츠와 함께 자연스럽게 영어를 학습할 수 있습니다.

<div style="margin-bottom:3rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/main_paris.jpg" oncontextmenu="return false;">
    <figcaption>메타버스로 구현된 파리</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/main_mission_talk1.jpg" oncontextmenu="return false;">
    <figcaption>영어 회화 학습</figcaption>
  </figure>
</div>

<details>
<summary class="my-underline" style="margin-bottom:1rem;">콘텐츠 플레이 영상</summary>
  <div class="my-media-row">
    <figure>
      <video controls muted style="width:70%;" poster="/assets/img/projects/lingocity/main_voca3_type1_thumbnail.jpg">
        <source src="/assets/img/projects/lingocity/main_voca3_type1_play.mp4" type="video/mp4">
      </video>
      <figcaption>학습 미션(Voca3 유형)</figcaption>
    </figure>
  </div>
</details>

<details>
<summary class="my-underline" style="margin-bottom:1rem;">공식 유튜브 영상</summary>
<div class="my-media-row">
  <figure class="youtube">
    <iframe src="https://www.youtube.com/embed/cw3pM-BBK2U" frameborder="0" allowfullscreen></iframe>
    <figcaption>티저 영상</figcaption>
  </figure>
  <figure class="youtube">
    <iframe src="https://www.youtube.com/embed/Law3TFX4kLY" frameborder="0" allowfullscreen></iframe>
    <figcaption>소개 및 설명 영상</figcaption>
  </figure>
</div>
</details>

<div style="margin-bottom:7rem;"></div>


## 특징
{:.my-heading-circle-1}

---

- Unity 3D
- 모바일 플랫폼(Android, iOS)
- STT, TTS를 활용한 영어 회화 학습
- ChatGPT를 결합한 영어 학습 콘텐츠
- API 서버

<div style="margin-bottom:7rem;"></div>


## 주요 담당 개발 사항
{:.my-heading-circle-1}

---

저는 링고시티에서 클라이언트 개발,<br>
그 중 콘텐츠 개발을 담당하며 링고시티 내 다양한 콘텐츠와 개발 보조 툴을 개발하였습니다.<br>
아래에는 주요 개발 내용들을 크게 세 가지로 구분하여 정리하였습니다.


### 게이미피케이션 콘텐츠
{:.my-heading-circle-2}
<!-- {:.my-heading-circle-2 style="margin-top:0.5rem;"} -->

> Gamification, 게임이 아닌 영역에 게임의 요소를 도입하여 사용자의 참여와 몰입도를 높이는 것
<!-- {:.message} -->

가장 주력으로 담당하여 개발한 분야입니다.<br>
링고시티에서의 재미 요소와 학습의 지속성을 높일 수 있는 방안에 대해 고민하였고,<br>
성장, 보상 등 다양한 게이미피케이션 요소를 활용한 콘텐츠를 개발하였습니다.

- **[버디(펫)]**<br>
- **[출석]**<br>
- **[랭크]**<br>
- **[아바타]**<br>
- **[감정 표현]**

[버디(펫)]: /lingocity/buddy.md
[출석]: /lingocity/attendance.md
[랭크]: /lingocity/rank.md
[아바타]: /lingocity/avatar.md
[감정 표현]: /lingocity/emotion.md

<div style="margin-bottom:3rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/main_buddy.jpg" oncontextmenu="return false;">
    <figcaption>버디</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/main_rank.jpg" oncontextmenu="return false;">
    <figcaption>리더보드</figcaption>
  </figure>
</div>


### 학습 미션
{:.my-heading-circle-2}

학습 미션은 링고시티의 학습 커리큘럼을 구성하는 메인 퀘스트 형태의 콘텐츠로<br>
유저들이 학습을 위해서 순차적으로 수행해야 하는 필수 콘텐츠입니다.<br>
다양한 유형으로 구성되어 있으며, 그 중 어휘 학습 유형의 마지막 유형인 'Voca3' 미션을 담당하여 개발하였습니다.
<!-- 유형 - voca 1,2,3 talk 1,2 find 1,2 medal photo freetalk pron chant -->

- **[Voca3 미션]**

[Voca3 미션]: /lingocity/voca3.md

<div style="margin-bottom:3rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/main_voca3_2.jpg" oncontextmenu="return false;">
    <figcaption>Voca3 미션</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/main_dashboard.jpg" oncontextmenu="return false;">
    <figcaption>학습 커리큘럼 & 미션들 </figcaption>
  </figure>
</div>


### 툴
{:.my-heading-circle-2}

팀 내 개발 효율성을 향상시키기 위한 개발 보조 툴을 개발하고 관리하였습니다.<br>
모바일 플랫폼 제품인 링고시티를 Windows 환경(개인 PC)에서 실행하고,<br>
인게임에서 여러 툴 기능을 활용하여 필요한 작업을 보다 효율적으로 진행할 수 있도록 개발하였습니다.

- **[링고시티 Windows 툴]**

[링고시티 Windows 툴]: /lingocity/wtt.md

<div style="margin-bottom:3rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/main_wtt.jpg" style="width:90%" oncontextmenu="return false;">
    <figcaption>툴 실행 장면</figcaption>
  </figure>
</div>

<div style="margin-bottom:7rem;"></div>


<!-- ## 갤러리
{:.my-heading-circle-1}

--- -->