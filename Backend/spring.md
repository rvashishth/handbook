### Core

- **ApplicationReadyEvent** - application is ready to serve requests
- **ApplicationFailedEvent** - Event published by a link SpringApplication when it fails to start
- **@RefreshScope** -

### Spring Boot

- `@EnableTransactionManagement` is not required to be declared in spring boot if the app already has spring-jdbc or spring-data in classpath. Plus spring-boot also adds `@Transactionl` on repository methods, so declaration of `@Transactional` is also not required in most of the time.

### Spring Cloud Gateway

- Route - id, destination uri, collection of filters, collection of predicates. Route only matches if aggregate result of predicates is true
- Predicate - takes `ServerWebExchange` as Generic type
- Filter - GatewayFilters each constructed using a specific factory. Can be pre,post or both.

> if Route uri doesn't define the port numbers, it takes the default one 443, 80

- Ways to configure route and filter predicate factories
  - Short hand (type and value)
    ```
    routes:
        - id: oauth-token
          uri: https://idxurl.com
          predicates:
            - Path=/oauth/token
    ```
  - Expanded
- Gateway Filter Factories
- Route Predicate Factories

  - After Route
  - Before Route
  - Between Route
  - Cookie Route
  - Header Route
  - Host Route
  - Method Route
  - Path Route
  - Query Route
  - Remote Address Route
  - Weight Route

- To add a filter and apply it to all routes, you can use `spring.cloud.gateway.default-filters`
- Alternative to application.properties we can also use `RouteLocatorBuilder` to define routes in Java
- In Link we have created a `GenericFilterFactory` by extending `AbstractGatewayFilterFactory`
- Filter vs Global filters
- **How to read request body.** </br>
  Request body has constraint that can only be read once per request.

  Ref - [SCG Unit Tests](https://github.com/spring-cloud/spring-cloud-gateway/tree/master/spring-cloud-gateway-core/src/test/java/org/springframework/cloud/gateway/filter/factory)
