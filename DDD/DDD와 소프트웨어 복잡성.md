## 도메인 주도 설계
* 소프트웨어 복잡성을 다루는 지혜
* 블루바이블
* 문제의 가치를 누구에게 둘 것인가?
* 관점차이에 따라서 문제의 핵심이 달라질 수 있다.

## 모델이란?
* 모델은 목적을 가지고 있다.
* 모델하우스 - 아파트 판매
* 현실세계에 있는 것을 가공하고 편집한다.
* 사용자가 관심을 가질 법한 것을 가공하고 편집한다.

## 도메인 모델
* 목적을 가진 의사소통 수단
* 개발 코드, 기획, 디자인 모든 것에 동일하게 사용되어야 함

## 유비쿼터스 랭귀지

## 전략적 설계
* 크게 소리 내어 모델링 하기
* 관심사 분리 격리 -> 집중할 범위를 정함

### 바운더리 컨테스트 (해결영역) 에 따라서 같은 도메인을 다룰때 다르게 취급할 수 있다.
* 하나의 팀이 여러개의 바운디드 컨텍스트를 다룰 수는 있음. 
* 하나의 바운디드 컨텍스트 여러 팀에서 다루면 안된다.

## 전술적 설계
* VALUE OBJECT
* ENTITY
* 애그리것
  * 갑객체나 엔티티를 하나의 군집으로 묶는 것
  * 동일한 라이프사이클 -> 불변식 혹은 비즈니스 규칙을 보장하는 단위 -> 일관성을 유지
  * 명령을 수행하기 위해 함꼐 조회되고 업데이트 해야하는 최소 단위
  * 외부에서 참조하는 녀석이 애그리것 루트가 된다.
  * 루트의 아이디 - 전역 식별자
  * 루트 아래의 애그리것의 아이디 - 지역 식별자
  * 다양한 시나리오 속에서 이럴땐 어떻게 되어야 하는가? 저럴땐 어떻게 되어야 하는가? 코드를 작성하는 것이 훨씬 빠를 수 있다.
* 리포지토리
* 도메인 서비스
  * 방파제 역할을 한다고 생각
  * 도메인 지식 혹은 불변식 이런것이 도메인 밖으로 새어나가는 것을 방지
* 팩토리
  * 팩토리 패턴에 대해서 생각하기 보다는
  * 생산자와 생성자 사이의 특별한 관계를 설명해줄 수 있음.
  * 
* 등...

## 전제 조건
* 개발자만을 위한 것이 아니다.

## Q&A
* 끝에 가서 나는 이렇게 생각했는데? 이런것을 어떤 방식으로...?
  * 글로 모델링 한것에 대해서 기록
  * 용어 사전을 지속적으로 관리

* 이벤트와 서비스호출 어떤 방식을 선택하는가?
  * 일관성 지키는 규칙만 잘 정리하는 것이 중요

* 작은 스타트업도 DDD 를 쓰는 것이 좋은가?
  * 도메인 전문가 없이 개발
  * 그럴바에는 일단, 빠르게 개발하고 나중에 리팩토링하는 것이 좋지 않겠는가?
  * 전술적 전략에 대해서는 권장
  * 유비쿼터스 랭귀지만 잘 지켜준다면 초석은 마련

* 도메인지식 전파하는 방법
  * 새로운 사람이 왔을때 온보딩 용도로 이벤트스토밍 하면서 갱신하는 것이 어떤가?

* 도메인을 포조로 해야한다.
  * DB 변경을 해본 경험은 있어도
  * JPA -> Mybatis x
  * 더 큰 동기부여가 되지 않으면 더 큰 설계를 하지 않는다.

* DDD를 객체지향언어로만 해야한다?
  * 그것은 아니라고 생각
  * 결국, 중요한건 유비쿼터스 랭귀지

* 모델드리븐 디자인 측면에서 텍스트로 정리한 도메인지식이 잘 매핑되지 않는데 그 것은 어떻게?
  * 가상 시나리오 상에서 지속적으로 검증하는 방식으로...
  * 100% 설계하고 구현하는 것은 아니기 때문에 지속적으로 수정이 되어야한다는 것을 모두 공감해야한다고 생각

* 팀 단위로 바운디드 컨텍스트? 아니다 도메인 다루다가 용어가 흐트러지면 바운디드 컨텍스트?
  * 처음에는 1팀으로 가다가 쪼개는 방식이 더 좋다고 생각?

* 혼자만 하는 개발하는 사람에 대해서는...?
  * 리눅스 만든 토발즈 급이면 그래도 된다...
  * 커뮤니케이션에 보다 참여할 수 있도록 이끌어야 한다는 생각...

* DDD 스타트 -> iDDD 반번흔 -> 도메인 주도 설계 -> 