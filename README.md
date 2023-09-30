 lol-searcher
==========================

해당 프로젝트는 인기 게임 '리그 오브 레전드' 의 유저별 게임 전적 및 게임 캐릭터 별 통계 자료 등을 제공해주는 애플리케이션입니다.

## 프로젝트 계기

> '리그 오브 레전드'라는 게임을 좋아해서 게임할 때 자주 애용하던 전적 검색 사이트가 있었습니다. 프로그래밍을 배우며 가장 친숙했던 사이트를 개발해보고자 하는 마음에서 시작된 프로젝트입니다.

## 프로젝트 목표

> - Open API 데이터를 소실없이 안전하게 저장 및 클라이언트로 전달
> - 애플리케이션 단에서 발생 가능한 어뷰징 및 웹 공격을 방지하도록 설계
> - 대용량 트래픽을 대비해 성능과 확장성을 고려한 아키텍처 설계 및 코드 작성

## 기능 요구 사항
> 1. 회원 가입, 회원 탈퇴, 회원 정보 조회 및 수정 기능
> 
> 2. 중복 로그인 허용, 회원 보안 등급에 따라 로그인시 등록된 이메일로 알림 및 2차 인증 기능 추가
> 
> 3. 게임 유저의 닉네임에 따른 게임 데이터 검색 기능(최신 순으로 정렬, 데이터 20개씩 페이징, 데이터베이스에서 조회)
> 
> 4. 특정 유저의 최신 게임 데이터 수집 기능(게임 서버의 Open API를 활용)
> 
> 5.  하룻동안 수집한 유저들의 게임 데이터로 특정 캐릭터, 아이템 등의 승률 통계 자료 추출
>     
> 6. 게임 캐릭터, 아이템별 통계 데이터에 따라 랭킹 매기는 기능
>
> 7. 과도한 갱신 요청 및 로그인 시도를 하는 어뷰저들에 대해 해당 서비스에 접근하지 못하도록 차단하는 기능

  
 ## 프로젝트 아키텍처
![image](https://github.com/kyo705/lol-searcher/assets/89891704/dac8c290-a91b-42ac-8d86-031ef1f999be)

 프로젝트 주요 컴포넌트
 ------------------
 
### [1. 웹 서버(Nginx)](https://github.com/kyo705/lol-searcher/wiki/Nginx)
  - 리버스 프록시 역할로 로드밸런싱, 요청 처리율 제한, 캐싱 용도로 사용

### [2. WAS1 : LOL-SEARCHER(Tomcat, Spring MVC)](https://github.com/kyo705/LolSearcher#lolsearcher)
  - 서블릿 기반 웹 애플리케이션 서버
  - 트랜잭션 보장이 필요한 서비스를 호출하는 용도
 
### [3. WAS2 : LOL-SEARCHER-REACTIVE(Netty, Spring Reactor)](https://github.com/kyo705/Lolsearcher-reactive#lolsearcher-reactive)
  - Java NIO 기반 이벤트 루프 웹 애플리케이션 서버
  - 빈번한 서버 간 통신이 필요한 서비스를 호출하는 용도
  - 게임 서버로부터 데이터를 수집하는 역할

### 4. 웹 보안(Spring Security)
  - WAS1, WAS2에 적용해 웹 보안 설정(CSRF, XSS, CORS, Session Fixation ...)

### [5. Kafka & Kafka Consumer Apps](https://github.com/kyo705/lol-searcher/wiki/Kafka-Consumer-Apps)
  - LOL-SEARCHER-REACTIVE에서 발생한 데이터를 필터링 및 가공해 DB에 저장하는 용도

### [6. Batch App](https://github.com/kyo705/lolsearcher-data-batch#lolsearcher-data-batch)
  - DB에 저장된 데이터를 가공해 DB 및 Redis(sorted set)에 저장
  
### 7. Notification Server(설계 예정)
  - 본인 인증 메일, 로그인 알림 등 다양한 알림 서비스를 제공하는 서버
  
