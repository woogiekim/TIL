# Overview

- 별 생각없이 사용하던 WebClient 의 Default Library에 대한 물음을 받았을 때 음..네티 였던 것 같은데... 잘모르겠습니다. 라고 대답할수 밖에 없었습니다.
- 그래서 직접 내부를 들어가 확인해보았습니다.

# WebClient Default Library

## 결론

- `netty —> jetty —> apache` 순으로 해당 라이브러리의 커넥터가 존재 유무를 확인해 `HttpClient` 를 생성합니다.

## WebClient 객체 생성 과정

- `WebClient.create()`을 하게되면`DefaultWebClientBuilder`를 이용해서`build`하게 됩니다.

```java
static WebClient create() {
    return (new DefaultWebClientBuilder()).build();
}
```

- `build`하는 과정에서`initConnector()`를 이용해`ClientHttpConnector`를 세팅합니다.

```java
@Override
public WebClient build() {
    ClientHttpConnector connectorToUse =
            (this.connector != null ? this.connector : initConnector());

    ExchangeFunction exchange = (this.exchangeFunction == null ?
            ExchangeFunctions.create(connectorToUse, initExchangeStrategies()) :
            this.exchangeFunction);

    ExchangeFunction filteredExchange = (this.filters != null ? this.filters.stream()
            .reduce(ExchangeFilterFunction::andThen)
            .map(filter -> filter.apply(exchange))
            .orElse(exchange) : exchange);

    HttpHeaders defaultHeaders = copyDefaultHeaders();

    MultiValueMap<String, String> defaultCookies = copyDefaultCookies();

    return new DefaultWebClient(filteredExchange, initUriBuilderFactory(),
            defaultHeaders,
            defaultCookies,
            this.defaultRequest, new DefaultWebClientBuilder(this));
}
```

- `initConnector()`에서`netty, jetty, apache`순으로 세팅합니다.

```java
private ClientHttpConnector initConnector() {
    if (reactorClientPresent) {
    return new ReactorClientHttpConnector();
    }
    else if (jettyClientPresent) {
    return new JettyClientHttpConnector();
    }
    else if (httpComponentsClientPresent) {
    return new HttpComponentsClientHttpConnector();
    }
    throw new IllegalStateException("No suitable default ClientHttpConnector found");
}
```