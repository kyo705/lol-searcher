 lol-searcher
==========================

해당 프로젝트는 인기 게임 '리그 오브 레전드' 의 유저별 게임 전적 및 게임 캐릭터 별 통계 자료 등을 제공해주는 애플리케이션입니다.

프로젝트 목표
------------------

 - 외부로부터 발생된 데이터를 소실없이 안전하게 저장 및 전달
 - 애플리케이션 단에서 발생 가능한 어뷰징 및 웹 공격을 방지하도록 설계
 - 대용량 트래픽을 대비해 성능과 확장성을 고려한 아키텍처 설계 및 코드 작성
 
 프로젝트 주요 컴포넌트
 ------------------
 
### 1. 웹 서버(Nginx)
  - 리버스 프록시 역할로 로드밸런싱, 요청 처리율 제한, 캐싱 용도로 사용

### 2. WAS1 : LOL-SEARCHER(Tomcat, Spring MVC)
  - 서블릿 기반 웹 애플리케이션 서버
  - 트랜잭션 보장이 필요한 서비스를 호출하는 용도
 
### 3. WAS2 : LOL-SEARCHER-REACTIVE(Netty, Spring Reactor)
  - Java NIO 기반 이벤트 루프 웹 애플리케이션 서버
  - 빈번한 서버 간 통신이 필요한 서비스를 호출하는 용도
  - 게임 서버로부터 데이터를 수집하는 역할

### 4. 웹 보안(Spring Security)
  - WAS1, WAS2에 적용해 웹 보안 설정(CSRF, XSS, CORS, Session Fixation ...)

### 5. Kafka & Kafka Consumer Apps
  - LOL-SEARCHER-REACTIVE에서 발생한 데이터를 필터링 및 가공해 DB에 저장하는 용도

### 6. Batch App
  - DB에 저장된 데이터를 가공해 DB 및 Redis(sorted set)에 저장
  
### 7. Notification Server(설계 예정)
  - 본인 인증 메일, 로그인 알림 등 다양한 알림 서비스를 제공하는 서버
  
  프로젝트 아키텍처
  ---------------
  ![image](https://user-images.githubusercontent.com/89891704/223715493-0e3d4626-a1f6-41e3-943c-24edb0dd65e8.png)
