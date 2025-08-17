---
layout: page
parent: lingocity
title: 출석
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
    <img src="/assets/img/projects/lingocity/attendance/attendance_1.jpg" oncontextmenu="return false;">
    <figcaption>출석 완료</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/attendance/attendance_2.jpg" oncontextmenu="return false;">
    <figcaption>출석 현황</figcaption>
  </figure>
</div>

* 목차
{:toc}


### 규칙

출석은 하루 한 번 접속 시 처리되며,<br>
출석 시 정해진 보상을 받고 연속해서 출석할수록 보상이 상향되는 방식으로 이루어져 있습니다.<br>
평일 5일 연속 출석 시 주간 보상을, 20일 연속 출석 시 월간 보상을 획득할 수 있습니다.<br>
보상은 20일을 기준으로 초기화되지만 연속 출석일 수는 계속해서 누적됩니다.<br>
출석하지 못한 날짜는 한 달에 최대 5번까지 구매할 수 있으며, 구매를 통한 출석도 보상과 연속 출석 일 수에 포함되도록 했습니다.


### 작동 방식

접속 시 가장 먼저 서버로부터 금일 출석 여부를 확인하고, 출석을 하지 않았다면 출석 처리 화면이 나타납니다.<br><br>
출석 처리는 지문을 확인하여 세이버스 요원임을 확인하는 스토리적 연출로 구성했으며(실제로 지문 확인을 하지는 않습니다.),<br>
지문 확인이 끝나는 순간 서버에 api를 요청하여 서버 측에서 출석을 처리할 수 있도록 하는 방식으로 개발하였습니다.<br><br>
해당 요청에 따른 서버 응답에는 보상, 연속 출석일 수, 출석 날짜 등이 포함되어 있고,<br>
이를 캐싱하여 이후 있을 보상 연출이나 달력 UI 등에 활용할 수 있도록 했습니다.

<div style="margin-top:3rem;"></div>
<div class="my-img-row">
  <figure>
    <img src="/assets/img/projects/lingocity/attendance/attendance_check_anim.gif" style="width:70%" oncontextmenu="return false;">
    <figcaption>출석 처리</figcaption>
  </figure>
</div>