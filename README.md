# project_description

### 어플리케이션 설명
이 어플레키션의 이름은 soccermate로, 축구 모임을 장려하기 위하여 개발한 앱입니다. 대표적인 기능은, 축구모임 생성, 거리에 기반하여 축구모임 추천, 앱내의 GPS기능을 사용하여 모임원의 출석인증입니다. 
더 자세한 사항은 아래의 프로젝트 링크를 참고해주시면 감사하겠습니다.

### 프로젝트 링크
- https://www.youtube.com/watch?v=nqBzfnXhum4

### 백앤드 아키텍처
- 본 프로젝트의 백앤드 아키텍처는 MSA를 사용하였습니다. 아래의 그림을 참고하여주시면 감사하겠습니다. 

<img width="1132" alt="스크린샷 2023-01-22 오후 3 07 21" src="https://user-images.githubusercontent.com/51771622/213903077-44848bdf-d92e-4ce4-83f1-85b6d2505023.png">

- 도메인을 6개(authentication, user, soccerGroup, announcement, vote, meeting)으로 나누었습니다. 
- 각 도메인은 완전히 독립되어야 하기때문에, 데이터베이스도 독립시켰습니다. 그 이유는 데이터베이스를 공유할시에 하나의 도메인에서 트래픽이 많이 나오면 데이터베이스에 성능에 영향을 미쳐서 다르 도메인도 영향을 미칠수 있기 때문입니다.
- Authentication과 user를 분리시켰습니다. 그 이유는 추후에 다른 인증방식이 추가되어도, 유저의 정보(닉네임, 사진 등등)은 영향을 미치지 않아야 한다고 생각이 들었습니다. 
- 미들웨어로는 kafka를 사용하여, microservice(도메인)끼리 비동기식으로 소통할수 있게하였습니다. 하지만, 가끔씩 유저의 요청을 처리하기 위해서, 동기식으로도 소통할수 있어야하는데, 이럴땐 openfeign을 사용하여 microservice들끼리 소통할수 있게 하였습니다. 
- ApiGateway는 로깅및 authorization역할을 합니다. 

#### Authentication
- 링크: https://github.com/soccermate/authentication
- 유저의 회원가입 및 그인 비번 찾기, 구글로 회원가입 및 로그인, 토큰 재발급 기능이 지원합니다.
- 기술 스택: Postgresql, Spring MVC, Spring Security, Oauth2 client, Spring kafka, AWS SES, jasypt, Spring jpa

#### User
- 링크: https://github.com/soccermate/user
- 유저의 정보(닉네임, 프로필사진, 주소)를 관리합니다.
- Spring Webflux를 사용하여, 쓰레드를 더욱더 효율적을 사용할수 있게끔 했습니다.
- 기술스택: Postgresql, AWS S3, Srping Webflux, ReactiveFeign, Spring Reactor, Reactor Kafka, jasypt, r2dbc

#### SoccerGroup
- 링크: https://github.com/soccermate/soccer_group
- 축구모임 생성, 조회, 가입요청 등의 기능이 있습니다.
- announcement, vote, meeting은 축구모임내에 있는 기능임에도 불구하고, 따로 나눈이유는 추후에 시간이 지나고 서비스를 운영함에따라서 축구모임내에 있는 기능들이 더 추가될것이기 때문입니다. 따라서, 확장하기 SoccerGroup 도메인과 그 안에있는 기능들을 다른 도메인으로 나누었습니다. 
- 기술스택: Postgresql, AWS S3, Srping Webflux, ReactiveFeign, Spring Reactor, Reactor Kafka, jasypt, r2dbc

#### Announcement
- 링크: https://github.com/soccermate/announcement
- 축구모임내의 공지 생성, 수정, 삭제 기능을 담당하는 도메인입니다.
- 기술스택: Postgresql, AWS S3, Srping Webflux, ReactiveFeign, Spring Reactor, Reactor Kafka, jasypt, r2dbc

#### Vote
- 링크: https://github.com/soccermate/vote
- 축구모임내의 투표 생성, 관리 기능을 담당하는 도메인입니다.
- 기술스택: Postgresql, AWS S3, Srping Webflux, ReactiveFeign, Spring Reactor, Reactor Kafka, jasypt, r2dbc

#### Meeting
- 링크: https://github.com/soccermate/meeting
- 축구모임내에서 언제 만날지 공지하고, 모임출석을 인증하는 도메인입니다.
- 기술스택: Postgresql, AWS S3, Srping Webflux, ReactiveFeign, Spring Reactor, Reactor Kafka, jasypt, r2dbc

#### ApiGateway
- 링크: https://github.com/soccermate/api_gate_way
- Proxy 서버입니다. 적절한 microservice에게 트래픽을 routing합니다.
- 기술스택:  AWS S3, Srping Webflux, ReactiveFeign, Spring Reactor, jasypt, spring-cloud-starter-gateway

#### Kubernetes
- 링크: https://github.com/soccermate/kubernetes
- 본 프로젝트는 AWS EKS를 사용하여, 컨테이너들을 관리하였습니다.
- 기술스택: AWS EKS, Route53, AWS RDS, AWS S3, AWS SES

### 팀원
- 백앤드: 정상원
- 프론트앤드: 유지은
- 디자인: 이상미