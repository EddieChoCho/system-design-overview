## Scaling
* Cannikin Law

### Auto-Scaling
* Auto-scaling should be used for stateless service only.
* Time of creating a node by auto-scaling might take 60 secs.  
* Issues could not be solved by auto-scaling:
    * Ecommerce system start to sell popular products -> million users login in few secs.
        * Solutions:
            * Cooperate with marketing team, extends the nodes in advance. 
            * Instead of panic buying, we could let user per-register the products or use lottery.
               
    * Happy new year issue (e.g. twitter)-> Users send requests at the same sec. 
    
### Scaling Stateful Services
* Should use [Consistent Hashing](http://michaelnielsen.org/blog/consistent-hashing/)
    * Old md5(md5(key)%n) hashing issues: if "n" change, total cache miss might be happened, DB might be overwhelmed by connections.
     
* Should not scaling the stateful services during peak hours.

## Rate-Limiting
* TDB
* Defense in depth
* Every layer needs rate-limiting 
    * Load balancer, e.g. [Rate Limiting with NGINX and NGINX Plus](https://www.nginx.com/blog/rate-limiting-nginx/)
    * App Server
    * Caching
    * Database
    
### Wait v.s. Reject 
* Rate-Limiting of each layer should has short waiting time.
    * If the waiting time is too long, user will try to make another request. (e.g. reload the page.)
    * If the waiting time is too long, please response HTTP 503.
     
* System resources will be occupied by the waiting task.
    * TCP/IP connection
    * Main Memory
    * CPU thread

### Thread Pool
* [Calculate the Optimum Number of Threads](http://baddotrobot.com/blog/2013/06/01/optimum-number-of-threads/)
* Open/close thread too frequently -> OS level overhead.
* Memory amount: "Waiting for worker to deal with the task". v.s. "Waiting CPU to deal with the thread(?)" 

### Database Connection Pool
* PHP has no connection pool issue: PHP-FRM architect(manager-worker pattern)
* Solution: Proxysql/pgpool

## Metrics
* Purpose: To check if anything goes wrong
* Resources usage(CPU, IO...)
* Number of exceptions/errors
* Various latencies
* Execution time of DB query
* [Observability and resiliency pattern in the cloud - Logging](https://github.com/EddieChoCho/tech-talks-note/blob/master/2019/ObservailityAndResiliencyPatternInTheColud.md#logging)

## Logging
* Purpose: Find bug
* Logrotate, store in AWS S3
* Build ELK
* Dump status of DB is also important(?)
* [Observability and resiliency pattern in the cloud - Metrics](https://github.com/EddieChoCho/tech-talks-note/blob/master/2019/ObservailityAndResiliencyPatternInTheColud.md#metrics)

## System tuning
* Premature optimization is the root of all evil.
* Need enough metric to find out the bottle neck.

### Hot Spot

* Hot spot is an issue of performance of single thread, it could not be solved by adding new nodes.
* Example:
    * If consistency is important to your system, no matter RMDB or NoSQL, 1 data stored in 1 mater node. 
    * A data is being updated frequently, the node witch stored that data will have huge IO.
    
#### Read Version Hot Spot
* Case: If a data store in a Redis node which will be read by all user, it will become the read version hot spot of the system.   
* Solution: MVCC, Snapshot would not be changed, so it could be stored in local cache.
* Tips: what is real time means to your users?

#### Write Version Hot Spot
* The solution for write version hot spot might be giving up the consistency?

## Blue-Green Deployment
 
## Reference
 * [Tech talks from Triton](http://github.com/TritonHo/slides/blob/master/Taipei%202019-10%20talk/concurrency.pdf)
 * [Observability and resiliency pattern in the cloud - by Sara Gerion](https://github.com/EddieChoCho/tech-talks-note/blob/master/2019/ObservailityAndResiliencyPatternInTheColud.md)

