# Redis分布式锁思考

为了确保分布式锁可用，需要确保锁的实现同时满足以下四个条件：

1.互斥性：任意时刻，只能有一个客户端获取锁，不能同时有两个客户端获取到锁。

2.安全性：锁只能被持有该锁的客户端删除，不能由其它客户端删除。

3.死锁：获取锁的客户端因为某些原因（如down机等）而未能释放锁，其它客户端再也无法获取到该锁。

4.容错：当部分节点（redis节点等）down机时，客户端仍然能够获取锁和释放锁。

利用Redis的Lua脚本来实现解锁操作的原子性

Redisson实现分布式锁的解决方案:

重点看下面这段Lua脚本

commandExecutor.evalWriteAsync(getName(), LongCodec.INSTANCE, command,
                  "if (redis.call('exists', KEYS[1]) == 0) then " +
                      "redis.call('hset', KEYS[1], ARGV[2], 1); " +
                      "redis.call('pexpire', KEYS[1], ARGV[1]); " +
                      "return nil; " +
                  "end; " +
                  "if (redis.call('hexists', KEYS[1], ARGV[2]) == 1) then " +
                      "redis.call('hincrby', KEYS[1], ARGV[2], 1); " +
                      "redis.call('pexpire', KEYS[1], ARGV[1]); " +
                      "return nil; " +
                  "end; " +
                  "return redis.call('pttl', KEYS[1]);",
                    Collections.<Object>singletonList(getName()), internalLockLeaseTime, getLockName(threadId));

1.采用的数据结构是hash,而不是传统的setnx

2.第一段if,客户端1尝试加锁，判断锁是否存在，如果不存在就加锁，并设置过期时间

3.第二段if,客户端2尝试加锁，如果锁存在,会判断当前锁的的hash结构中是否包含客户端2的ID,显然不存在，客户端2会获取到当前锁的一个剩余过期时间，之后进入while循环，不断尝试加锁

4.客户端1的默认加锁时间是30秒，如果超过这个时间，会有一个后台线程，每10秒检查一下，如果当前客户端还持有这个锁，就自动延长过期时间，确保长时间的业务场景能够在锁存在的时间内执行完

5.可重入锁，如果客户端1第二次尝试加锁，会在hash结构中，将ID数字自增1，如果释放锁，这个ID减1，到0就说明这个锁已经释放了

##### 会带来的问题

如果在主从集群的环境中，当master节点当机后，会将其锁信息同步到slave节点，会导致多个客户端同时完成加锁。

