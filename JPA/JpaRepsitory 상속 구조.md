# JpaRepository 란?

- 내가 생각해 본 결론은 Spring에서 JPA를 사용하기 편리한 형태로 맞춰 놓은 스펙이라고 생각합니다.
- 어떤 점을 편리하게 해놓은 것인지 알아보는 시간을 가져보기로 합니다.
- `JpaRepository` 는 `PagingAndSortingRepository` 와 `QueryByExampleExecutor` 를 상속하고 있다. 그렇다면, 두 인터페이스는 어떤 내용이 있는가에 대해서 알아봅니다.

# JpaRepository 의 상속구조

## Repository 는 마킹을 위한 인터페이스입니다.

```java

@Indexed
public interface Repository<T, ID> {

}
```

## CrudRepository 는 Repository 를 상속합니다.

```java

@NoRepositoryBean
public interface CrudRepository<T, ID> extends Repository<T, ID> {

    <S extends T> S save(S entity);

    <S extends T> Iterable<S> saveAll(Iterable<S> entities);

    Optional<T> findById(ID id);

    boolean existsById(ID id);

    Iterable<T> findAll();

    Iterable<T> findAllById(Iterable<ID> ids);

    long count();

    void deleteById(ID id);

    void delete(T entity);

    void deleteAllById(Iterable<? extends ID> ids);

    void deleteAll(Iterable<? extends T> entities);

    void deleteAll();
}
```

- CRUD 를 할 수 있는 method 들을 제공해주는 것을 알 수 있습니다.

## JpaRepository 는 PagingAndSortingRepository 와 QueryByExampleExecutor 를 상속합니다.

```java

@NoRepositoryBean
public interface JpaRepository<T, ID> extends PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {

    @Override
    List<T> findAll();

    @Override
    List<T> findAll(Sort sort);

    @Override
    List<T> findAllById(Iterable<ID> ids);

    @Override
    <S extends T> List<S> saveAll(Iterable<S> entities);

    void flush();

    <S extends T> S saveAndFlush(S entity);

    <S extends T> List<S> saveAllAndFlush(Iterable<S> entities);

    @Deprecated
    default void deleteInBatch(Iterable<T> entities) {deleteAllInBatch(entities);}

    void deleteAllInBatch(Iterable<T> entities);

    void deleteAllByIdInBatch(Iterable<ID> ids);

    void deleteAllInBatch();

    @Deprecated
    T getOne(ID id);

    T getById(ID id);

    @Override
    <S extends T> List<S> findAll(Example<S> example);

    @Override
    <S extends T> List<S> findAll(Example<S> example, Sort sort);
}
```

- `flush` 류 method 와 `batch` 류 method 그리고 `getById` (`getOne` 은 `@Deprecated` 처리 되었습니다.) 를 추가적으로 제공하고 있습니다.

# PagingAndSortingRepository 란?

- `PagingAndSortingRepository` 는 `CrudRepository` 를 상속하고 있고, 두 가지 method 를 정의해놓고 있습니다.

```java

@NoRepositoryBean
public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID> {

    Iterable<T> findAll(Sort sort);

    Page<T> findAll(Pageable pageable);
}
```

- 페이징과 소팅 처리를 할 수 있도록 두 가지 method를 제공하는 것을 알 수 있습니다.

# QueryByExampleExecutor 란?

- Example 객체를 이용해 손쉽게 쿼리 할 수 있게 도와줍니다.

```java
public interface QueryByExampleExecutor<T> {

    <S extends T> S findOne(Example<S> example);

    <S extends T> Iterable<S> findAll(Example<S> example);

    // … more functionality omitted.
}
```

## QueryByExampleExecutor 의 장점

- 쿼리를 이용하지 않더라도 심플하게 쿼리를 할 수 있습니다.

## QueryByExampleExecutor 의 단점

- 복잡한 쿼리를 사용하는 것에는 적합하지 않은 것 같습니다.

  - 이런 식으로 subtype에서 오버라이딩 할 때에 return type은 supertype 을 구현하고 있다면, return type으로 허용된다.

```java
public class IntegerProducer extends Producer {
    @Override
    public Integer produce(String input) {
        return Integer.parseInt(input);
    }
}
```