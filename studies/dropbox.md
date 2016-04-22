## Design a high-availability file sync system like Dropbox
An excellent talk is here: https://www.youtube.com/watch?v=PE4gwstWhmc

### Supported Operations
1. User should be able to upload/download any file from his/her pc to the service
2. Users should be able to sync their entire repo on the service with their pc
3. Regular CRUD operations (Create, Read, Update, Delete) operations should be allowed on the repo. Results of these operations must be immediately available to syncing service (i.e. modifying a file on server and then syncing it must return modified version to the pc and overwirte the stale version).
4. ACID operations: 
   * Atomic: File upload should be all or none 
   * Consistency: Both versions on pc and server must be same
   * Isolation: ?
   * Durability: Must be highly available

### Scale of the Problem
1. 10 million users
2. 100 million requests per day
3. Very high write/read ratio (almost 1:1)

### Abstract Architecture
1. Read-Write App Servers
   * A sync operation happens on read-write server
2. Push servers
   * A CRUD operation takes place on one push server. The results of the operation are pushed back to the client
3. Database
   * NoSQL database to store file objects
4. Memcache
   * All app servers periodically write their results to memcache
5. Load Balancers
   * All requests to app servers go through load balancers
6. File Stores for backup

### Operations

### Bottlenecks