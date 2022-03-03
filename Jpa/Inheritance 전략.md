# Inheritance 전략

## SINGLE_TABLE

### 특징

- 기본 설정
- 테이블 하나에 Sub Class 들을 Type 으로 구분

### 단점

- 사용하지 않는 빈 컬럼들이 너무 많이 생겨날 수 있음
- Sub Class 들의 제약이 DB 에 포함되기 어려울 수 있음
  - nullable 하게 해줘야 하기 때문에... 너무 러프한 설정으로 혼란을 줄 수 있음 

```sql
create table Animal (
    id serial,
    dtype char(30),
    ...
);
```

## JOINED

### 특징

- 테이블을 정규화 한다

### 단점

- 전체 조회를 하려면, 조인을 해야 하기 떄문에 쿼리가 복잡해질 수 있음
- 조인으로 인한 성능저하가 있을 수도 있기는 하지만 보통 미비함

```sql
create table Animal (
    id serial,
    name varchar(100),
    ...
);

create table Tiger (
    id serial,
    animal_id int,
    ...
);

create table Lion (
    id serial,
    animal_id int,
    ...
);
```

## TABLE_PER_CLASS

```sql
create table Tiger (
    id int,
    name varchar(100)
    ...
);

create table Lion (
    id int,
    name varchar(100)
    ...
);
```

### 특징

- JOINED 전략과 비슷하다.
- 중복되는 컬럼을 제거하고 상위 테이블에서 관리하는 JOINED 와는 다르게 컬럼 관리를 하위 테이블에서도 한다.
- 상위 테이블이 생성되지 않는다.

### 단점

- ID 생성 전략을 TABLE 로 해야 한다는 제약이 있다.
- 전체 조회를 하려고 하면, union all 을 해야 하기 때문에 성능 문제가 있다.