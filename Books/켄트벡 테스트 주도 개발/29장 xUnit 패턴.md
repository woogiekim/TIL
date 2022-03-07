# xUnit 패턴

## 단언(assertion)

- 예전에 `assertTrue(rectangle.area() != 0)`와 같은 혀앹의 단언을 본 적이 있다.
- 이 경우 0이 아닌 아무 값이나 반환하게 만들어도 조건을 만족하므로 이 단언은 별로 유용하지 않다.
- 단언은 구체적이어야 한다.
- 만약 면적(area)이 50이어야 한다면, 다음과 같이 면적이 50이어야 한다고 적어주어야 한다. `assertTrue(rectangle.area() == 50)`
- 객체를 블랙박스처럼 취급하기란 쉽지 않다.
- 청약됨(Offered) 혹은 시행중(Running)이라는 상태(Status)를 가질 수 있는 계약(Contract)에 대한 테스트를 작성한다고 생각해보자.
- 아마 다음과 같이 작성할 것이다.

```java
Contract contract = new Contract(); // 디폴트로 Offered status
```