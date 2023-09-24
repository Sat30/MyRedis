
#### About myRedis
- myRedis is a in-memory Data store that draws inspiration from `Redis`, a renowned key-value store known for its speed, simplicity, and flexibility.
- Just like Redis, myRedis is designed to handle a wide array of data storage and retrieval tasks efficiently.
- It offers a wide range of features and use cases, making it an indispensable tool for developers and businesses seeking high-performance data management solutions.
  
##### Core Asprects
 - Developed a project which involves the transport  layers of the TCP/IP model.
 - Also in application layer involves Data serialization and custom protocol.
 - The project primarily emphasized `Data Structures` and `Network concepts`.

##### Non Blocking IO using Event Loop
- Single threaded server should handle multiple clients.
- A single-threaded process concurrently monitors multiple file descriptors for events(read, write) using the `poll()` function."
- In this scenario, each file descriptor (socket) represents a client connection.
- After performing the `poll()` operation, we initiate `read()` or `write()` operations exclusively on the available file descriptors.


##### Our Custom Protocol
![image](https://github.com/Sat30/MyRedis/assets/101095981/319074a1-139a-456e-87fb-b9252bfbb080)



- Used **Type Length Value (TLV) Scheme** to do **Data serialization** 
- Implemented Protocol Parser for the this TLV scheme

##### Data Structure to Implement Key-value store with fast lookup
- Redis is primarily an in-memory key-value database, but it offers exceptional flexibility as it allows values to be of any data type
- In my Redis value can be primitive Data structure or String or Sorted Set
- **Selecting and designing a in memory data structure  to efficiently store key-value pairs while ensuring fast lookup.**

![image](https://github.com/Sat30/MyRedis/assets/101095981/42372a9a-7b4e-4c0c-9a27-2af77106bf60)


###### Hash Table :
- Employed Open Hashing (Chaining) to manage collisions when two keys (`key1` and `key2`) produce the same hash value `(hash(key1) == hash(key2))`.
- Implemented `Progressive Resizing` to mitigate time complexity concerns, allowing the hash table to dynamically adjust its size as needed for optimal performance.
- Implemented `Intrusive Linked Lists` to link collided key-value pairs within the hash table.
- Intrusive Linked Lists to minimize `Cache thrashing` and improve cache locality by incorporating linked list nodes directly into the data structure, enhancing performance.

###### Entity (Key - value) :
- Entity Represent `Key - Value Pair`
- Value Can `Primitive Data Structute` or `Sorted Set`

###### Sorted Set 
- Sorted Set same as `STL set` but have some extra functionality
- Elements in a sorted set are ordered or sorted based on their associated scores.
- Sorted sets allow for efficient range queries.  The Client can retrieve a subset of elements within a specified range of scores


##### Timer
- The Redis server must manage timeouts for idle TCP connections effectively.
- In our server code, the only blocking operation is poll().
- To facilitate kicking out idle TCP connections, we set the poll() timeout value to the nearest timer associated with a connection. 
- To determine the nearest timer, we utilize a straightforward linked list that maintains the order of timers. Whenever a new timer is added or an existing one is updated, it is placed at the end of the list
  
![image](https://github.com/Sat30/MyRedis/assets/101095981/b770ddaf-abc6-4767-a188-1abf702cad5f)



##### TTL
- Time To Live for particular `key`

###### Application of myRedis
- **If value Type is (`int` or `String`)**
  
 1. `Rate Limiting`: Implement rate limiting or throttling for API requests. Store counters in myRedis to control the rate at which requests are processed.
 2. `Session Management`: Store session data for web applications. This can include user sessions, shopping cart contents, and other session-related information.
  
- **If value Type is `Sorted Set`**
  
1. `Leaderboards and Rankings` : Use sorted sets to maintain leaderboards for games or rankings for users, products, or content based on scores. Scores can represent points, ratings, or other relevant metrics.
2. `Recommendation Systems`: Implement recommendation engines where items are ranked based on user preferences or item popularity. The sorted set allows you to quickly retrieve top recommendations.
