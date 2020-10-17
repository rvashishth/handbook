- [SpockFramework](http://spockframework.org/) - Unit Testing
- Gradle
- OkHttp
- [Orika Java Bean Mapping](https://orika-mapper.github.io/orika-docs/)
- [Resilience4J](https://github.com/resilience4j/resilience4j) - circuit breaker
- [Hystrix] - circuit breaker
- [Spring framework](./spring.md)
- [Reactor Flux Mono](./spring-reactor.md)
- [Verbal RegEx](http://verbalexpressions.github.io/)

#### Reference Reads

- [Spock Crash Course](http://spockframework.org/spock/docs/1.0/interaction_based_testing.html)

Groovy Goodness

- tap https://mrhaki.blogspot.com/2018/06/groovy-goodness-easy-object-creation.html
- with https://mrhaki.blogspot.com/2009/09/groovy-goodness-with-method.html
- collect - iterate through list to generate a new list
- transform list into map `Map<Integer,App> result = apps.collectEntries {[it.id, it]}`
- [Safe navigation operator `person.?name`](https://docs.groovy-lang.org/latest/html/documentation/core-operators.html#_safe_navigation_operator)

Groovy blogs

- https://mrhaki.blogspot.com/

Unit Test

- test name format should be as below </br>
  `'<method-name> does something' and '<method-name> does something when <some condition>'`
