# Iterable vs Reactive (Duality)

- Iterable 과 Reactive 는 상대적인 관계 있음
- 프로그래밍에서 상대적이라는 것은 서로의 같은 행동을 하지만
- 서로의 형태가 반대인 상황을 일컫는다 

# Observable 의 단점

- 마지막 데이터에 대한 complete 이런 부분에 대한 프로토콜이 없음 
  - 이러면 언제 끝났는지 알수가 없음 물론 로직으로 구현하면 되긴 하지만, 보장이 안됨
- 예외가 발생되었을때 처리에 대한 프로토콜이 없음
  - 그러니까 복구 가능한 예외 같은 것 예외가 밸상했지만 재시도를 한다던가
  - 다른 행동을 취한다던가 그런 부분이 녹아져 있지 않음

# Reactive 의 표준 인터페이스

- Processor
- Publisher
  - 무제한으로 연속된 요소들을 제공하는 것
  - onSubscribe onNext* (onError | onComplete)?
  - onSubscribe --> 반드시 호출
  - onNext* --> zero ~ infinity
  - onError or onComplete --> optional
- Subscriber
- Subscription

# Backpressure

# 왜 필요한가?

- Pub 과 Sub 의 속도차이가 있을 수 있는데, 이런 부분을 해결하기 위함

# Publisher

- 무제한으로 연속된 요소들을 제공하는 것
- 데이터 스트림을 계속해서 제공하느 프로바이더 역할을 함 

## Publisher 가 제공하는 인터페이스
기본적으로 Publisher 는 1가지의 인터페이스를 제공해야 한다

- subscribe*
  - Subscriber 를 등록한다

# Subscriber

- Publisher 가 제공하는 요소들을 구독해서 어떤 행동을 한다 

## Subscriber 가 제공하는 인터페이스

기본적으로 Subscriber 는 4가지의 인터페이스를 제공해야 한다

  - onSubscribe --> 반드시 호출해야 한다
  - onNext* --> Zero ~ Infinity
  - onError  --> optional
  - onComplete --> optional

# Subscription

WIP