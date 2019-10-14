# Caching

### Application server cache
### Content Distribution Network (CDN) [3]

Serving content from CDNs can significantly improve performance in two ways:

* Users receive content at data centers close to them
* Your servers do not have to serve requests that the CDN fulfills
##### Push CDNs [2]
Push CDNs receive new content whenever changes occur on your server. You take full responsibility for providing content, uploading directly to the CDN and rewriting URLs to point to the CDN. You can configure when content expires and when it is updated. Content is uploaded only when it is new or changed, minimizing traffic, but maximizing storage.

Sites with a small amount of traffic or sites with content that isn't often updated work well with push CDNs. Content is placed on the CDNs once, instead of being re-pulled at regular intervals.

##### Pull CDNs [2]
Pull CDNs grab new content from your server when the first user requests the content. You leave the content on your server and rewrite URLs to point to the CDN. This results in a slower request until the content is cached on the CDN.

A time-to-live (TTL) determines how long content is cached. Pull CDNs minimize storage space on the CDN, but can create redundant traffic if files expire and are pulled before they have actually changed.

Sites with heavy traffic work well with pull CDNs, as traffic is spread out more evenly with only recently-requested content remaining on the CDN.

##### Disadvantage(s): CDN
* CDN costs could be significant depending on traffic, although this should be weighed with additional costs you would incur not using a CDN.
* Content might be stale if it is updated before the TTL expires it.
* CDNs require changing URLs for static content to point to the CDN.
### Cache Invalidation [3]
* `Write-through cache`: Data is written into the cache and the corresponding database at the same time. 
  * The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, 
we will have complete data consistency between the cache and the storage. 
  * It ensures that nothing will get lost in case of a crash, power failure, or other system disruptions. (minimizes the risk of data loss)
  * It has the disadvantage of higher latency for write operations. (every write operation must be done twice before returning success to the client)

* `Write-around cache`: 
  * Data is written directly to permanent storage, bypassing the cache. 
  * This can reduce the cache being flooded with write operations that will not subsequently be re-read, 
  * It has the disadvantage that a read request for recently written data will create a “cache miss” and must be read from slower back-end storage and experience higher latency.

* `Write-back cache`: 
  * Data is written to cache alone and completion is immediately confirmed to the client. 
  * The write to the permanent storage is done after specified intervals or under certain conditions. 
  * Low latency and high throughput for write-intensive applications.
  * It has risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.

### Cache eviction policies [3]
* First In First Out (FIFO): The cache evicts the first block accessed first without any regard to how often or how many times it was accessed before.
* Last In First Out (LIFO): The cache evicts the block accessed most recently first without any regard to how often or how many times it was accessed before.
* Least Recently Used (LRU): Discards the least recently used items first.
* Most Recently Used (MRU): Discards, in contrast to LRU, the most recently used items first.
* Least Frequently Used (LFU): Counts how often an item is needed. Those that are used least often are discarded first.
* Random Replacement (RR): Randomly selects a candidate item and discards it to make space when necessary.

### Best practices
* Redis: Cache eviction should not depend on LRU but TTL. [4]
    * Without setting TTL, Redis cache eviction(LRU) might be happened during peak hours!
        * And it will be force eviction when writing new data
        * And Redis is single thread, one task could delay all queue
    * Redis LRU algorithm is not an exact implementation - [Approximated LRU algorithm](https://redis.io/topics/lru-cache)

### Reference
* [1][What is Distributed Caching? Explained with Redis!](https://youtu.be/U3RkDLtS7uY) (Description for write-back cache is incorrect.)
* [2][The System Design Primer](https://github.com/donnemartin/system-design-primer/blob/master/README.md#content-delivery-network)
* [3][Grokking The System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)
* [4][Tech talks from Triton](http://github.com/TritonHo/slides/blob/master/Taipei%202019-10%20talk/concurrency.pdf)