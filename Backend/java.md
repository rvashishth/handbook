- Predicate(test, and, or, negate, isEqual)
  - https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html
  - https://mkyong.com/java8/java-8-predicate-examples/
- Java 8 Stream class
  - use Stream.generate method to generate an stream of infinite elements.
    ```
    Stream<Date> dates = Stream.generate( () -> new Date());
    dates.forEach(System.out::println);
    ```
    to control the number of outputs
    ```
    Stream<Date> dates = Stream.generate( () -> new Date()).limit(10);
    dates.forEach(System.out::println);
    ```
- org.springframework.util.MultiValueMap allows to store list of values in a map as `Map<K, List<V>>`
