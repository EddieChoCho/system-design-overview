# Caching

### Application server cache
### Content Distribution Network (CDN)
### Cache Invalidation
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

### Cache eviction policies
* First In First Out (FIFO): The cache evicts the first block accessed first without any regard to how often or how many times it was accessed before.
* Last In First Out (LIFO): The cache evicts the block accessed most recently first without any regard to how often or how many times it was accessed before.
* Least Recently Used (LRU): Discards the least recently used items first.
* Most Recently Used (MRU): Discards, in contrast to LRU, the most recently used items first.
* Least Frequently Used (LFU): Counts how often an item is needed. Those that are used least often are discarded first.
* Random Replacement (RR): Randomly selects a candidate item and discards it to make space when necessary.

### Reference
* [What is Distributed Caching? Explained with Redis!](https://youtu.be/U3RkDLtS7uY)(Description for write-back cache is incorrect.)
* [Grokking The System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)