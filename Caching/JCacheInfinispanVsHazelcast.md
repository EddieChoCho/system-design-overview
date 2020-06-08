# JCache - Infinispan v.s Hazelcast

## JCache / Java Cache / JSR 107
* JCache uses the top-level package name of javax.cache, and defines the following five core interfaces:
    * Cache. This defines access to the actual cache, which is a map-like data structure that stores key-value pairs.
    * Entry. This defines access to the key-value pairs stored in the cache.
    * CacheManager. This defines how caches are managed.
    * CachingProvider. This defines how CachingManagers are managed.
    * ExpiryPolicy. This defines how expiration is handled for entries.

* Two defined mechanisms in which an entry can be stored in a cache.
    * Store-by-value(default): 
        * The key-value pairs are stored in the cache, and new copies of the entries are made and returned when accessed from the cache. 
    * Store-by-reference(optional): 
        * The cache stores and returns reference to application-provided key-value pairs. 
        * This lets updates to the application-provided key-value pairs to be seen in subsequent accesses without having to update the cache entries themselves.
        
## Infinispan

### Why would I use Infinispan?
* Most people use Infinispan for one of two reasons.
    * As a cache
        * Putting Infinispan in front of your database, disk-based NoSQL store or any part of your system that is a bottleneck can greatly help improve performance. 
        * Often, however, a simple cache isn't enough - for example if your application is clustered and cache coherency is important to data consistency. A distributed cache can greatly help here.
    * As a high-performance NoSQL data store
        * In addition to being in memory, Infinispan can also persist data to a more permanent store. We call this a cache store. 
        * Cache stores are pluggable, you can easily write your own, and many already exist for you to use.

* Another common use case is adding clusterability and high availability to frameworks. 
* Since Infinispan exposes a distributed data structure, frameworks and libraries that also need to be clustered can easily achieve this by embedding Infinispan and delegating all state management to Infinispan. 
* This way, any framework can easily be clustered by letting Infinispan do all the heavy lifting.

## Infinispan vs Hazelcast (TBD)
* http://109.239.60.130/compare/jboss-infinispan/vs/redis-database
* https://hazelcast.com/resources/benchmark-infinispan/

## References
* [JCache / Java Cache](https://hazelcast.com/glossary/jcache-java-cache/)
* [Introduction to Infinispan](https://infinispan.org/about/)
