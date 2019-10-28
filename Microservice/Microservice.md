# Microservice

### Fault Tolerance
* TBD
### Resilience
* TBD

### Async/Sync Communication between Services
* Issues of using http only for communications 
    * Could lead to tight run time coupling services.
    * Multiple points of failures.
    * The more services were involved in the request, the longer response time will be.

### Two Phase Commit in Microservices
* To atomically update the database and send out the message
#### [Choreography-Based Saga Pattern](https://microservices.io/patterns/data/saga.html)
* [Event Sourcing](https://microservices.io/patterns/data/event-sourcing.html)
* [Transactional Outbox Pattern](https://microservices.io/patterns/data/transactional-outbox.html)
    1. Insert the message to a outbox table. So that just done atomically.
    2. Retrieve that message from the outbox table. and publishing it to the message broker.

### API gateway 
* Similar to facade pattern

### References
* [Episode 370: Chris Richardson on Microservice Patterns](https://www.se-radio.net/2019/06/episode-370-chris-richardson-on-microservice-patterns/)
* [Microservice Patterns](https://microservices.io/index.html)