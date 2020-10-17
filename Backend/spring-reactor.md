https://projectreactor.io/docs/core/3.3.5.RELEASE/api/index.html
Best documentation is under Flux, Mono classes

- **Flux** is a reactive stream publisher
- reactive stream is a push model where as java 8 stream is based on pull model
- **Backpressure** when a consumer/subscriber can tell producer how much data it can handle
- **ConnectableFlux** won't emit data on subscribe, instead it allow multiple subscribers to join.

Open Items

- [ ] hot stream/sequence vs cold sequence

Reference

- https://stackabuse.com/spring-reactor-tutorial/
