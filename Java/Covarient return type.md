# The Covariant return type 이란?

- OOP 프로그램에서 Covariant return type이라는 것은 서브 클래스에서 오버라이딩 할 때 더 좁은 타입으로 대체하는 것을 이야기합니다. 자바에서는 5.0 버전에서부터 지원하기 시작했다고 합니다.

- 아래는 위키피디아에서 발췌한 설명입니다.

  > In object-oriented programming, a covariant return type of a 
  > method is one that can be replaced by a “narrower” type when the
  > method is overridden in a subclass. A notable language in which 
  > this i a fairly common paradigm is C++. C# supports return type 
  > covariance as of version 9.0. Covariant return type have been 
  > partially allowed in the Java language since the release of 
  > JDK5.0, so the following example wouldn’t compile on a previous 
  > release:
  > 
  > *by  Wikipedia*

- Baeldung 에서 발췌한 설명입니다.

  > Covariant can be considered as a contract for how a subtype is accepted when only then supertype is defined. 
  > 
  > The covariant return type is - when we override a method - what
  > allows the return type to be the subtype of the type of the
  > overridden method.
  > 
  > *by Baeldung*

# The Covariant return type 예시 코드

- 슈퍼타입 클래스가 이런식으로 있다고 생각했을 때

```java
public class Producer {
    public Object produce(String inpuit) {
        Object result = inpuit.toLowerCase();
        return result;
    }
}
```

- 이런식으로 subtype 에서 오버라이딩 할 때에 return type은 supertype 을 구현하고 있다면, return type으로 허용 된다.

```java
public class IntegerProducer extends Producer {
    @Override
    public Integer produce(String input) {
        return Integer.parseInt(input);
    }
}
```

# The Covariant return type이 갖는 의미란?

- 일단, Covariant return type 은 subtype 이 supertype 과 is-a 관계를 만족해야 한다는 리스코프 치환 법칙과 연관이 있습니다. “*어떻게 return type 이 subtype 이여도 허용해 줄 수 있는가?”* 라는 생각을 하면, subtype 은 supertype 을 완벽하게 대체할 수 있어야 하기 때문입니다.
- 이렇게 subtype으로 return type 이 올 수 있는 것은 다운그레이드 하게 되면, 조금 더 구체적이게 해주기 때문에 사용하는 입장에서는 훨씬 분명한 목적을 가지고 메서드를 이용할 수 있습니다. 위에 예시 코드만 보아도 Integer type 을 produce 하는 행위를 한다는 것이 명확해지고 더 안정적으로 메서드를 사용할 수 있게 되었다는 점이 Covariant return type 이 의미하는 바라고 생각합니다.