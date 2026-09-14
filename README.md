# FatDog AI 플랫폼 V2 - JPA & 도커 가상화 🐳🤖

<!-- workspace-readme-learning:start -->
## 파일과 연결한 학습 안내

아래 설명은 이 폴더의 실제 소스와 빌드 설정을 기준으로 정리했습니다. 기존 소개의 기능 설명은 연결된 파일과 함께 확인할 수 있습니다.

### 주요 파일과 역할

| 파일 | 역할과 읽을 내용 |
| --- | --- |
| [Dockerfile](<Dockerfile>) | 컨테이너 이미지의 빌드·실행 단계 |
| [pom.xml](<pom.xml>) | Maven 의존성·플러그인·패키징 설정 |
| [src/main/java/org/example/fatdogai2/controller/ChatController.java](<src/main/java/org/example/fatdogai2/controller/ChatController.java>) | 요청 매핑·입력 바인딩과 응답 처리 — `index`, `chat`, `clear` |
| [src/main/java/org/example/fatdogai2/FatDogAi2Application.java](<src/main/java/org/example/fatdogai2/FatDogAi2Application.java>) | Spring Boot 애플리케이션 진입점 — `main` |
| [src/main/java/org/example/fatdogai2/repository/ChatMemoryJpaRepository.java](<src/main/java/org/example/fatdogai2/repository/ChatMemoryJpaRepository.java>) | Spring Data의 엔티티 저장·조회 계약 |
| [src/main/java/org/example/fatdogai2/repository/JpaChatMemoryRepository.java](<src/main/java/org/example/fatdogai2/repository/JpaChatMemoryRepository.java>) | 데이터 저장·조회 인터페이스 또는 구현 — `findConversationIds`, `findByConversationId`, `saveAll` |
| [src/main/java/org/example/fatdogai2/service/AIChatService.java](<src/main/java/org/example/fatdogai2/service/AIChatService.java>) | 업무 처리와 외부 의존성 호출 — `chat` |
| [src/main/java/org/example/fatdogai2/service/ChatService.java](<src/main/java/org/example/fatdogai2/service/ChatService.java>) | 업무 처리와 외부 의존성 호출 — `chat`, `clearHistory` |
| [src/main/java/org/example/fatdogai2/config/ChatClientConfig.java](<src/main/java/org/example/fatdogai2/config/ChatClientConfig.java>) | 빈 등록 또는 외부 설정 구성 — `groqChatClient`, `geminiChatClient`, `nimChatModel` |
| [src/main/java/org/example/fatdogai2/config/ChatMemoryConfig.java](<src/main/java/org/example/fatdogai2/config/ChatMemoryConfig.java>) | 빈 등록 또는 외부 설정 구성 — `chatMemory` |
| [src/main/java/org/example/fatdogai2/domain/ModelProvider.java](<src/main/java/org/example/fatdogai2/domain/ModelProvider.java>) | Java 타입과 동작 정의 |
| [src/main/java/org/example/fatdogai2/domain/NimProperties.java](<src/main/java/org/example/fatdogai2/domain/NimProperties.java>) | 빈 등록 또는 외부 설정 구성 — `NimProperties`, `Chat` |
| [src/main/java/org/example/fatdogai2/dto/ChatDTO.java](<src/main/java/org/example/fatdogai2/dto/ChatDTO.java>) | 입력·응답 데이터의 구조 — `ChatDTO` |
| [src/main/java/org/example/fatdogai2/entity/BaseEntity.java](<src/main/java/org/example/fatdogai2/entity/BaseEntity.java>) | 구현체가 따라야 하는 인터페이스 |
| [src/main/java/org/example/fatdogai2/entity/ChatMessageJPA.java](<src/main/java/org/example/fatdogai2/entity/ChatMessageJPA.java>) | DB 테이블과 대응하는 영속 엔티티 — `toMessage`, `fromMessage` |
| [src/main/java/org/example/fatdogai2/ServletInitializer.java](<src/main/java/org/example/fatdogai2/ServletInitializer.java>) | Java 타입과 동작 정의 |
| [src/main/webapp/WEB-INF/views/index.jsp](<src/main/webapp/WEB-INF/views/index.jsp>) | FatDog Chat - 스마트 인공지능 어시스턴트 화면 — 서버 모델을 표시하는 JSP |
| [src/test/java/org/example/fatdogai2/FatDogAi2ApplicationTests.java](<src/test/java/org/example/fatdogai2/FatDogAi2ApplicationTests.java>) | 테스트 코드 |

### 실행과 설정 확인

- [pom.xml](<pom.xml>)의 의존성과 패키징을 기준으로 구성합니다. 선언된 Java 설정은 17입니다.
- 저장소 루트에서 `.\mvnw.cmd spring-boot:run`으로 Spring Boot를 실행합니다.
- 환경 설정: [src/main/resources/application-dev.properties](<src/main/resources/application-dev.properties>), [src/main/resources/application.properties](<src/main/resources/application.properties>).
- 코드·설정에서 참조하는 환경 변수 이름: `GEMINI_API_KEY`, `GROQ_API_KEY`, `NIM_API_KEY`. 기본값과 필수 여부는 각 참조 위치에서 확인합니다.

### 관련 PDF와 보충 설명

- [7/28 강의](<../260629_ex/새 폴더/7-28/README.md>): 프롬프트·구조화 출력·Advisor·대화 저장소를 연결합니다.
- [7/23 강의](<../260629_ex/새 폴더/7-23/README.md>): 엔티티·영속성 컨텍스트·연관관계와 N+1을 연결합니다.

이 링크는 구현을 이해하기 위한 관련 기초 자료입니다. 해당 강의가 이 저장소의 모든 기능이나 이후 버전의 API를 설명한다는 뜻은 아닙니다.

### 읽는 순서와 복습

- 대화 식별자 → 이력 조회 → 모델 호출 → 메시지 저장의 흐름을 확인합니다. 메시지 역할·순서·소유자 접근을 검증하고 구조화 결과의 필수값을 따로 확인합니다.
- Entity와 Repository에서 시작해 Service의 트랜잭션 및 연관 객체 접근을 읽습니다. 변경 감지 시점, 지연 로딩과 SQL 횟수, DTO 변환의 경계를 확인합니다.

테스트 소스가 포함되어 있습니다. 이 문서 수정 작업에서는 애플리케이션·DB·외부 API 테스트를 실행하지 않았으므로 실행 결과를 보장하는 기록은 아닙니다.

<!-- workspace-readme-learning:end -->

스프링 부트 환경에서 데이터베이스 데이터 모델 클래스인 JPA 엔티티를 활용하고, 인공지능 API 호출 기능과 도커(Docker) 컨테이너 패키징 설정을 함께 엮어 배포 준비 과정을 훈련하는 고급 프로젝트입니다.

---

## 📂 학습 파일 구성 (Files)

- [pom.xml](<pom.xml>) : Spring Data JPA, AI 드라이버, Lombok 라이브러리 종속성 설정
- [Dockerfile](<Dockerfile>) : 서버를 리눅스 컨테이너 가상 환경에 패키징하여 빌드하는 도커 파일
- [scratch/merge_jpa.ps1](<scratch/merge_jpa.ps1>) : JPA 관련 설정을 병합 제어해 주는 유틸 스크립트

---

## 🛠 배운 핵심 개념 (What We Learned)

- **JPA 데이터 영속성**: SQL 문을 매번 짜지 않고 자바 객체와 테이블을 1:1 자동 맵핑하여 데이터를 조작하는 ORM 핵심 기술을 공부합니다.
- **컨테이너 가상화 (Docker)**: 개발 컴퓨터뿐만 아니라 어떤 환경에서도 서버가 정상 구동되도록 가상 환경의 종속성을 이미지로 조립하는 개념을 익힙니다.

---

## 🚀 실행 및 확인 방법 (How to Run)

1. `.env.dev.example` 파일을 참고하여 개발용 설정 환경변수 `.env.dev`를 만듭니다.
2. 도커가 켜진 상태에서 `docker build -t fatdog-ai-2 .` 명령으로 이미지를 생성하거나, IDE에서 직접 Spring Boot 서비스를 띄웁니다.

<!-- pdf-til-supplement:start -->
## TIL 부연 설명 — PDF와 연결하기

기존 실습 내용을 이해하기 위한 PDF 기반 부연 설명이다. 아래 예시는 개념을 설명하기 위한 것이며, 이 프로젝트에서 실행해 관찰한 결과와는 구분한다. 페이지 번호는 표지를 포함한 PDF 순서다.

함께 읽을 파일: [src/main/java/org/example/fatdogai2/controller/ChatController.java](<src/main/java/org/example/fatdogai2/controller/ChatController.java>) · [src/main/java/org/example/fatdogai2/FatDogAi2Application.java](<src/main/java/org/example/fatdogai2/FatDogAi2Application.java>) · [src/main/java/org/example/fatdogai2/repository/ChatMemoryJpaRepository.java](<src/main/java/org/example/fatdogai2/repository/ChatMemoryJpaRepository.java>)

### 구조화 출력과 대화 메모리는 별도 문제

구조화 출력은 모델 응답을 앱에서 다루기 쉬운 타입으로 변환하는 과정이다. JSON으로 파싱되었다고 내용까지 맞는 것은 아니므로 필수 값과 범위를 검증해야 한다. 대화 메모리는 이전 메시지를 다시 실어 보내며 conversationId로 대화를 구분한다.

**예시로 이해하기:** 일정 결과의 날짜·장소 필드가 존재해도 실제로 가능한 일정인지는 별도 검증 대상이다. 대화 ID를 받는 API는 그 ID가 현재 사용자의 것인지 확인해야 한다. 메모리 저장소에 기록했다는 사실과 모델 요청에 이력이 포함되었다는 사실도 구분한다.

근거: 331-2 Spring AI 활용 — [11쪽](<../260629_ex/새 폴더/7-28/331-2_Spring_AI_활용.pdf#page=11>) · [13쪽](<../260629_ex/새 폴더/7-28/331-2_Spring_AI_활용.pdf#page=13>) · [27쪽](<../260629_ex/새 폴더/7-28/331-2_Spring_AI_활용.pdf#page=27>) · [29쪽](<../260629_ex/새 폴더/7-28/331-2_Spring_AI_활용.pdf#page=29>) · [33쪽](<../260629_ex/새 폴더/7-28/331-2_Spring_AI_활용.pdf#page=33>)

### 영속 상태와 변경 감지

JPA는 엔티티와 테이블의 매핑을 정의하는 표준이고 Hibernate는 이를 구현한다. 영속성 컨텍스트가 관리하는 엔티티의 변경은 flush 시점에 SQL로 반영될 수 있다. 객체 필드를 바꾸는 즉시 DB에 커밋되는 것은 아니며 flush와 commit도 같은 뜻이 아니다.

**예시로 이해하기:** 트랜잭션 안에서 조회한 엔티티의 상태를 바꾸는 흐름과 화면에서 받은 새 객체를 save하는 흐름을 구분한다. 쓰기 트랜잭션 밖의 객체 변경이 자동 저장된다고 가정하지 않는다. 엔티티를 응답에 직접 노출하기보다 필요한 값을 DTO에 담으면 저장 구조와 응답 계약을 분리할 수 있다.

근거: 323-1 JPA와 Hibernate — [31쪽](<../260629_ex/새 폴더/7-23/323-1_JPA와_Hibernate.pdf#page=31>) · [33쪽](<../260629_ex/새 폴더/7-23/323-1_JPA와_Hibernate.pdf#page=33>) · [35쪽](<../260629_ex/새 폴더/7-23/323-1_JPA와_Hibernate.pdf#page=35>) · [42쪽](<../260629_ex/새 폴더/7-23/323-1_JPA와_Hibernate.pdf#page=42>) · [46쪽](<../260629_ex/새 폴더/7-23/323-1_JPA와_Hibernate.pdf#page=46>)

### 연관관계의 주인과 N+1의 발생 시점

양방향 관계에서는 외래키 변경을 반영하는 연관관계의 주인이 중요하다. 반대쪽 컬렉션에만 추가하면 기대한 FK 변경이 저장되지 않을 수 있다. LAZY는 필요한 시점까지 조회를 미루지만 반복문에서 연관 객체를 하나씩 읽으면 N+1 쿼리가 생길 수 있다.

**예시로 이해하기:** 회원 목록 1회 조회 후 각 회원의 팀을 읽으며 추가 SQL이 발생하는지 확인한다. fetch join이나 조회 전용 DTO로 필요한 데이터를 가져오는 방법을 비교한다. 컬렉션 fetch join과 페이징을 함께 쓰면 행 수가 늘어나므로 단순히 한 번의 쿼리로 줄이는 것만 목표로 삼지 않는다.

근거: 323-2 JPA 연관관계 매핑과 N1 문제 — [9쪽](<../260629_ex/새 폴더/7-23/323-2_JPA_연관관계_매핑과_N1_문제.pdf#page=9>) · [11쪽](<../260629_ex/새 폴더/7-23/323-2_JPA_연관관계_매핑과_N1_문제.pdf#page=11>) · [19쪽](<../260629_ex/새 폴더/7-23/323-2_JPA_연관관계_매핑과_N1_문제.pdf#page=19>) · [21쪽](<../260629_ex/새 폴더/7-23/323-2_JPA_연관관계_매핑과_N1_문제.pdf#page=21>) · [26쪽](<../260629_ex/새 폴더/7-23/323-2_JPA_연관관계_매핑과_N1_문제.pdf#page=26>) · [28쪽](<../260629_ex/새 폴더/7-23/323-2_JPA_연관관계_매핑과_N1_문제.pdf#page=28>)

<!-- pdf-til-supplement:end -->

<!-- infra-pdf-20260914:start -->
## TIL 부연 설명 — 9월 인프라 PDF

기존 실습을 새로 추가된 PDF와 연결해 풀어 쓴 설명이다. 페이지 번호는 표지를 포함한 PDF 순서이며, 아래 개념 예시는 실제 실행 결과와 구분한다.

### 이미지 빌드와 컨테이너 실행은 다른 단계

현재 [Dockerfile](<Dockerfile>)은 `pom.xml`을 먼저 복사해 의존성을 준비하고 소스를 복사한 뒤 WAR를 만든다. 최종 JRE 이미지에서는 `java -jar app.war`로 실행한다. 따라서 모든 WAR를 직접 실행할 수 있다고 일반화하지 말고, 빌드가 실행 가능한 WAR로 재패키징하는지 `pom.xml`의 구성을 함께 읽어야 한다. `-DskipTests`를 사용하므로 패키징과 테스트 검증도 구분한다.

멀티 스테이지는 컴파일에 필요한 도구와 운영 시 필요한 실행 파일을 분리하는 방식이다. 앞 단계에서 만든 파일 중 `COPY --from`으로 선택한 것만 다음 단계로 옮긴다. `docker build`는 이미지를 만들며 웹 서버를 계속 실행해 두는 명령은 아니다. 실제 서비스는 그 이미지로 컨테이너를 생성·실행할 때 시작된다. 빌드 성공 후에도 런타임의 DB 접속·환경변수·포트 문제로 시작에 실패할 수 있다.

PDF 근거: 이미지 빌드·볼륨·네트워크 — [4쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=4>) · [5쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=5>) · [6쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=6>) · [7쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=7>)

### 설정·공개 포트·저장 데이터의 경계

이 Dockerfile의 `ENV PORT=8080`만으로 Spring의 `server.port`가 자동 변경된다고 단정할 수 없다. 애플리케이션 설정에서 PORT를 읽는지 확인한다.

`EXPOSE`는 사용 포트를 이미지에 명시하는 것이며 호스트 포트를 실제로 여는 동작은 아니다. `-p 호스트포트:컨테이너포트` 또는 Compose의 `ports`가 두 포트를 연결한다. 컨테이너 안의 `localhost`는 그 컨테이너 자신이므로 별도 DB 컨테이너를 찾는 주소로 사용할 수 없다. 같은 사용자 정의 네트워크에 연결된 컨테이너는 이름으로 상대를 찾는 구성을 사용할 수 있다.

이미지에는 실행 코드를 두고 환경별 값은 실행 시 전달하면 같은 이미지를 여러 환경에서 사용할 수 있다. 업로드·DB 파일처럼 재생성 후에도 남아야 하는 데이터는 컨테이너 쓰기 레이어와 분리한다. 볼륨은 영속 저장의 수단이며 백업 자체를 대신하지는 않는다.

PDF 근거: 이미지 빌드·볼륨·네트워크 — [10쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=10>) · [11쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=11>) · [12쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=12>) · [13쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=13>) · [14쪽](<../260629_ex/새 폴더/9-8/03-1_도커_이미지_빌드와_볼륨_네트워크.pdf#page=14>)

<!-- infra-pdf-20260914:end -->
