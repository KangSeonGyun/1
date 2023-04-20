---
title: Spring Security, Filter
tags: spring
---

## Spring Boot와 Spring Security 연동

ID/PW 기반의 로그인 처리 학습   
세션(HttpSession) 기반의 예제로 사용자 정보는 서버에서 보관하고, 필요한 경우 설정을 통해 제어하도록 구성했다.

스프링 시큐리티 동작에는 여러개위 객체가 서로 데이터를 주고 받으면서 이루어 지며 기본 구조는 다음과 같다.

<img src="/assets/images/spring-security-1.png" title="참고 이미지" alt="이미지" />

핵심 역할은 Authentication Manager를 통해 이루어진다. Authentic

참고한 책 : 코드로 배우는 스프링 부트 웹 프로젝트 (남가람북스)
