## 最大内存容量：Maximum Memory
- 64 bit：无限制
- 32 bit：3 GB
> Large memory can contain more data and increase hit ratio, one of the most important metrics but at a certain limit of memory the hit rate will be at the same level.

## 缓存淘汰算法：Eviction algorithms
### LRU：Last Recently Used
- 最近使用过的 key，将来还会再次被使用
- 在一段长的闲置时间内命中一次，key 会在下次淘汰周期才会被缓存
> track when key was used last time. So probably it will be still used in future, but what if it was only ‘one shot’ before long idle time? Key will be stored to next eviction cycle.

### LFU：Least Frequently Used
- 从 Redis 4.0 开始支持
- 计算 key 使用次数
- 最常使用的 key 会在淘汰周期下生存下来

###### 问题
- 当一个 key 只在前一段时间经常使用
- 另一个 key 开始被请求，但使用次数仍然比较少的时候，key 会被列入候选淘汰
###### 解决方法：
- 长时间生存的 key， 会在一定淘汰周期内，减少次数

> count how many times key was used. The most popular keys will survive eviction cycle. Problem appears when key was used very often some time ago. Another key starts to being requested but it still have smaller counter and will be candidate to eviction (Redis team solved problem with long lived keys by decreasing counter after some period of time).


## 持久化策略：Durability
- 如果热身准备不需要，关闭所有持久化选项
- 每一个额外的选项会消耗 Redis 运算力

> Every additional fork or other operations like fsync() will consume power, 
if warming-up is not important, disable all persistence options.

### RDB
##### 间隔快照
- 在一段时间间隔之间快照
- 在一定写请求次数之后快照

> point-in-time snapshots after specific interval of time or amount of writes. Rare snapshots should not harm performance but it will be a good task trying to find balance between snapshot interval and to avoid serving obsolete data after outage.

### AOF
- 对每次写操作，记录在持久化日志中
- 如果执行这一级别的持久化策略，应该阅读，在appendfsync配置参数下，fsync 策略的区别

> create persistence logs with every write operation. If you consider this level of durability, you should read about different fsync policies under appendfsync configuration parameter.

### RDB + AOF




---
###### 资料来源
- [Spring Boot cache with Redis – Matthew Frank – Medium](https://medium.com/@MatthewFTech/spring-boot-cache-with-redis-56026f7da83a)








