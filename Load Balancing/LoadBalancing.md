# Load Balancing
Load Balancer (LB) is another critical component of any distributed system. 
It helps to spread the traffic across a cluster of servers to improve responsiveness and availability of applications, websites or databases. 
LB also keeps track of the status of all the resources while distributing requests (Health Checks). 
If a server is not available to take new requests or is not responding or has elevated error rate, 
LB will stop sending traffic to such a server.

### Places could be added Load Balancers
* Between the user and the web server
* Between web servers and an internal platform layer, like application servers or cache servers
* Between internal platform layer and database.

### Load Balancing Algorithms
* `Least Connection Method` — This method directs traffic to the server with the fewest active connections. This approach is quite useful when there are a large number of persistent client connections which are unevenly distributed between the servers.
* `Least Response Time Method` — This algorithm directs traffic to the server with the fewest active connections and the lowest average response time.
* `Least Bandwidth Method` - This method selects the server that is currently serving the least amount of traffic measured in megabits per second (Mbps).
* `Round Robin Method` — This method cycles through a list of servers and sends each new request to the next server. When it reaches the end of the list, it starts over at the beginning. It is most useful when the servers are of equal specification and there are not many persistent connections.
* `Weighted Round Robin Method` — The weighted round-robin scheduling is designed to better handle servers with different processing capacities. Each server is assigned a weight (an integer value that indicates the processing capacity). Servers with higher weights receive new connections before those with less weights and servers with higher weights get more connections than those with less weights.
* `IP Hash` — Under this method, a hash of the IP address of the client is calculated to redirect the request to a server.

### Further Reading
* [Using nginx as HTTP load balancer](http://nginx.org/en/docs/http/load_balancing.html)
### Reference
* [What is Load Balancing? (by Gaurav Sen)](https://youtu.be/K0Ta65OqQkY)
* [Grokking The System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)
