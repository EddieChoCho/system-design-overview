# RDBMS

### Connection Pool

### MVCC
* Oracle, PostgreSQL  
* TBD

### SXLock
* MySQL, DB2
*TBD

### Best Practices
#### Should not let third-party service connect to your RDBMS directly.
* Potential Issues: 
    * If the third-party service leaves a lot of connections open, it will lead to a performance issues to our system. 
    * If the third-party service opens and closes connections frequently without using connection pool, it will lead to a performance issues to our system.
    * If the third-party service starts a transaction and keeps it open for a long period. 
    The old version data could not be cleaned by the MVCC operator, 
    (MVCC operator will keeps the data which might be used by the transaction) which lead to massive undo log.
    (Note: PostgreSql will not use undo log but keep the data of old version in tablespace.)
    * Although we could set read-only permission to the third-party service. 
    The [row level write lock](https://riptutorial.com/mysql/example/24166/row-level-locking) still could be get by the service 
    if it uses `SELECT ... FOR UPDATE`. 
    * The bugs in the third-party service might become DDoS attacks to your database.
    
* Conclusion: Third-party service could only get data from our RDBMS through APIs of our web service. 
So ee could prevent the potential issues and have more options to handle rate-limiting issues.  

### References
* [Tech talks from Triton - RDBMS](https://github.com/TritonHo/slides/tree/master/Taipei%202019-04%20course)
* [Backend TW](https://www.facebook.com/groups/616369245163622/permalink/1768723876594814/)      




