# Zookeepr
A important system for maintaining data across your entire cluster in a consistent manner.
## What is Zookeeper?
1. It basically keeps track of information that must be synchronized across your cluster.
  * Which node is the master?
    * What tasks are assigned to which workers?
    * Which workers are currently available?
2. It's a tool that applications can use to recever from partial failures in your cluster.
3. An integral part of HBase, High-Availability (HA) MapReduce, Drill, Storm, Solr and much more.

## Failure modes
* Master crashes, needs to fail over to a backup
* Worker crashes - it's work needs to be redistributed
* Network trouble - part of your cluster can't see the rest of it.


## "Primitive" operations in a distributed system
1. Master election
    * One node registers itself as a master, and holds a "lock" on that data.
    * Other nodes cannot become master until that lock is released
    * Only one node allowed to hold tha lock at a time.
2. Crash detection (!)
    * "Ephemeral" data on a node's availability automatically goes aways if the node disconnects, ot fails to refresh itself after some time-out period.
3. Group management (!)
4. Metadata (!)
    * List of outstanding tasks, task assignments.

* But Zookeepr's API is not about these primitives.
* Instead they have built a more general purpose system that makes it easy for applications to implement them.

## Zookeeper's API
1. Really a little distributed file system.
    * With strong consistency guarantees
    * Replace the concept of "file" with "znode" and you've pretty much got it.
2. Here's the Zookeeper API (!)
    * Create, delete, exists, setData, getData, getChildren

## Notifications (!)
* A client can register for notifications on a zonde
    * Avoids continuous polling
    * Example: register for notification on/master - if it goes away, try to take over as the new master.

## Persistent and ephemeral znodes (!) (usage for diffent node, one service will have multiple node?)
1. Persistent znodes remain stored until explicitly deleted
    * e.g. assigment of tasks to workers must persist event if master crashes
2. Ephemeral znodes go away if the client that created it crashes or loses its connection to ZooKeeper
    * e.g. if the master crashes, it should release its lock on the znode that indicates which node is the master.
