### 1.分布式session问题

> 因此在分布式环境下同一个用户发送的多次HTTP请求可能会先后落到不同的服务器上，导致后面发起的HTTP请求无法拿到之前的HTTP请求存储在服务器中的Session数据，从而使得Session机制在分布式环境下失效。

解决方案:'

1. Session集中式存储:比如说存储在分布式缓存Redis集群中。其原理是以SessionId作为Key，序列化后的HttpSession对象作为Value存储在Redis中，然后将SessionId返回给客户端，当下一次客户端发送HTTP请求到服务器的时候，会带上这个SessionId，服务器再根据SessionId从Redis拿到相应的Session数据并反序列化成HttpSession对象
2. Session复制:当客户端在某台服务器上存储了Session数据的时候，我们可以手动地将该Session信息同步到集群中的其他服务器上面去，这样一来所有服务器就都存储了所有客户端的Session信息了，因此即使同个客户端的多次HTTP请求落到不同的服务器上面去，也还是能够拿到相应的Session信息;但是这样占用空间增大；
3. Session Sticky:如果能够保证同个客户端发起的HTTP请求都由相同的服务器进行处理，那么也可以避免Session失效的问题

4. Cookie Based：存储在Cookie中，这样一来服务器就无需维护客户端的状态，即服务器“无状态化”，无状态化的好处是利于服务器水平扩展；但是如果用户禁用cookie将会变得不一样；

### 2.负载均衡策略

https://www.jianshu.com/p/d7e173d212a8

1. 轮询：实现简单，请求均匀分配；不适合长连接和命中率有要求的场景
2. 加权轮询：适合无状态的场景，权值需要静态配置，无法自动调节。也不适合对长连接和命中率有要求的场景。
3. 随机
4. 哈希
5. 最小连接数：把请求分配给活动连接数最小的后端服务器。它通过活动来估计服务器的负载。比较智能，但需要维护后端服务器的连接列表。
6. 加权最小连接数:加权最小连接数(Weighted Least Connection)，在后端服务器性能差异较大的情况下，可以优化LC的性能，高权值的服务可以承受更多的连接负载。
7. 最短响应时间：把请求分配给平均响应时间最短的后端服务器。平均响应时间可以通过ping探测请求或者正常请求响应时间获取。
    RT(Response Time)是衡量服务器负载的一个非常重要的指标。对于响应很慢的服务器，说明其负载一般很高了，应该降低它的QPS
8. 一致性哈希

### 分布式锁的实现

#### 分布式锁的特点

1. 互斥性:和我们本地锁一样互斥性是最基本，但是分布式锁需要保证在不同节点的不同线程的互斥。

2. 可重入性:同一个节点上的同一个线程如果获取了锁之后那么也可以再次获取这个锁。

3. 锁超时:和本地锁一样支持锁超时，防止死锁。

4. 高效，高可用:加锁和解锁需要高效，同时也需要保证高可用防止分布式锁失效，可以增加降级。

5. 支持阻塞和非阻塞:和ReentrantLock一样支持lock和trylock以及tryLock(long timeOut)。

6. 支持公平锁和非公平锁(可选):公平锁的意思是按照请求加锁的顺序获得锁，非公平锁就相反是无序的。这个一般来说实现的比较少。

#### mysql分布锁

#### redis分布锁

### 分布式追踪的上下文是怎么存储和传递的

