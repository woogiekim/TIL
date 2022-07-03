# Spring Batch

## 핵심 포인트
- 일괄처리 되는 기능이 100번째 순서에서 실패할 경우 101번째 부터 다시 시작할 수 있을까?

```
어떤식으로 구성하느냐에 따라 달라질 수는 있지만,
- 여러가지 Step을 넣어서 나열하는 상황을 순서라고 이야기할 수도 있고
- 일괄처리 횟수를 순서라고 이야기 할 수도 있음.

물론, 특정 조건에 만족 할 때까지 여러번 반복할 수 있게 설정할 수도 있지만 RepeatStatus를 FINISHED로 한다는 가정

여러 Step이라고 가정했을 경우 Job을 재시도 하더라도 기본적으로 이미 완료한 Step을 다시 실행하지 않기 때문에 보장이 됨

일괄처리 횟수인 경우에도 이미 완료한 Job의 파라미터로 다시 시도하려고하면, 예외가 발생하기 때문에 Parameter만 잘 조정해주면 됨
```

- 이미 실행 된 작업을 다시 실행할 수 없게 막을 수 있을까?

```
물론, 특정 조건에 만족 할 때까지 여러번 반복할 수 있게 설정할 수도 있지만 RepeatStatus를 FINISHED로 한다는 가정

실행해서 완료 된 작업의 JobExecution의 상태는 COMPLETED로 기록 되고
스프링 배치는 COMPLETED된 작업을 다시 실행하려고 하면, 예외가 발생 한다.
그러므로, 이미 실행 된 작업을 다시 실행할 수 없게 막을 수 있고, JobRepository를 통해 기록 되기 떄문에 파악할 수 도 있다.

JobParameter를 어떻개 설정 하느냐가 중요하다. (잘못 설정해놓으면, 한 번만 실행 되어야 하는데 여러번 실행 될 수도 있음)
```

- 실행 중에 예외가 발생되서 실패한 경우 재시도 할 수 있을까?

```
작업 실행 중에 실패하게 되면 실패한 Step은 FAILED로 처리 되고
FAILED 상태인 Step들만 실행 되기 때문에 재시도 할 수 있다.
```


## 스프링 배치 도메인

### Job
- 스프링 배치에서 가장 상위에 있는 도메인이다.
- 말그대로 작업에 해당하는 `Job` 그 자체를 이야기 한다고 볼 수 있다.
- 반드시 1개 이상의 `Step`으로 구성되어 있어야 한다.
- 기본 구현체
  - SimpleJob - 순차적으로 `Step`을 실행
  - FlowJob - 특정 조건과 흐름에 따른 `Step`을 구성해 실행

#### JobInstance
- `Job`이 실행될 때 생성되고, 고유하게 식별 가능한 작업 실행 단위이다.
- `JobInstance` 생성 전략
  - 처음 시작하는 `Job`과 `JobParameter` -> 새로 생성
  - 동일한 `Job`과 `JobParameter`로 실행 -> `JobName`과 `JobKey`로 `JobInstance` 조회해 객체 획득
- 기본적으로 `BATCH_JOB_INSTANCE` 테이블에 저장 됨

#### JobExecution
- `JobInstance`를 한 번 시도했을 때 저장
- 실행 상태 전략
  - COMPLETED - 재실행 불가
  - FAILED - 재실행 가능
- 기본적으로 `BATCH_JOB_EXECUTION` 테이블에 저장 됨
- `JobInstance` (1) -> (*) `JobExecution`

#### JobParameter
- `Job`을 실행할 때 포함해 사용하는 파라미터를 가짐
- 파라미터의 타입은 `String`, `Date`, `Double`, `Long` 등을 지원
- 기본적으로 `BATCH_JOB_EXECUTION_PARAMS` 테이블에 저장 됨
- `JobExecution` (1) -> (*) `JobParameter`

### Step
- `Job`의 처리를 정의하고 컨트롤하는 모든 정보를 가지고 있는 도메인
- `Job`을 어떻게 구성하고 실행한 것인지 Task 기반으로 설정하고 명세 해놓을 수 있다.
- 모든 `Job`은 하나 이상의 `Step`으로 구성 됨

#### StepExecution
- `Step`을 한 번 시도했을 때 저장
- `Job`을 재시작해도 이미 성공해서 완료된 `Step`은 재실행 되지 않고, 실패한 `Step`만 실행 됨
- `StepExecution`이 모두 완료 되어야 `JobExecution`이 완료 됨
- 기본적으로 `BATCH_STEP_EXECUTION` 테이블에 저장 됨
- `JobExecution` (1) -> (*) `StepExecution`
