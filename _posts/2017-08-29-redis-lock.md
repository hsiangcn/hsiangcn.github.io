---  
layout: post  
title: Redis实现分布式锁  
categories: REDIS  
description: 使用redis实现分布式锁
keywords: java、Redis  
--- 

实现原理很多大神已经说的很清楚了，话不多说直接上源码

## 1. Redis命令介绍
### 1.1 [SETNX](http://redisdoc.com/string/setnx.html)
**SETNX key value**<br>
将 key 的值设为 value ，当且仅当 key 不存在。<br>
若给定的 key 已经存在，则 SETNX 不做任何动作。<br>
SETNX 是『SET if Not eXists』(如果不存在，则 SET)的简写。<br>
返回值：<br>
设置成功，返回 1 。<br>
设置失败，返回 0 。<br>
### 1.2 [EXPIRE](http://redisdoc.com/key/expire.html)
**EXPIRE key seconds**<br>
为给定 key 设置生存时间，当 key 过期时(生存时间为 0 )，它会被自动删除。<br>
返回值：<br>
设置成功返回 1 。<br>
当 key 不存在或者不能为 key 设置生存时间时(比如在低于 2.1.3 版本的 Redis 中你尝试更新 key 的生存时间)，返回 0 。<br>
### 1.3 [DEL](http://redisdoc.com/key/del.html)
**DEL key [key ...]**<br>
删除给定的一个或多个 key 。<br>
不存在的 key 会被忽略。<br>
返回值：<br>
被删除 key 的数量。<br>

## 2. 源码

```
    /**
     *  获取锁
     *
     * @param key	锁的key
     * @param timeout	获取锁的等待时间，单位： 毫秒
     * @param expire	获取锁后设置锁的有效时间，单位：秒
     * @return
     */
    public void lock(String key, long timeout, int expire) {
        boolean lock = false;

        try {
            // 为防止锁key出现重复，在key的后面增加lock
            String lockkey = key + "&&lock";
            long nanoTime = System.nanoTime();
            timeout = timeout * 1000 * 1000;
            // 在timeout的时间范围内不断轮询锁,超过timeout的时间没有获取锁则抛出异常
            while (timeout == 0 || (System.nanoTime() - nanoTime) <= timeout) {
                if (lockSource(lockkey, expire) == 1) {
                    // 抢锁成功
                    lock = true;
                    break;
                }
                logger.info("  >>>>>  出现锁等待 <<<<<<< ");
                //短暂休眠，避免可能的活锁
                Thread.currentThread().sleep(50);
            }
        } catch (Exception e) {
            throw new RuntimeException("locking error", e);
        }

        if (!lock) {
            new RuntimeException(" >>>>>>>>  抢锁失败  <<<<<<<<<<  ");
        }

    }
    
    /**
     *  锁 资源
     *
     * @param key
     * @param expire 获取锁后设置锁的有效时间，单位：秒
     * @return 1 成功，0加锁失败
     */
    private int lockSource(String key, int expire) {
        ShardedJedis shardedJedis = shardedJedisPool.getResource();
        long i = shardedJedis.setnx(key, key);
        if (i == 1L && expire > 0) {
            i = shardedJedis.expire(key, expire);
            return (int) i;
        } else {
            // 如果key没有设置超时时间，将当前超时时间是指到key上
            if (shardedJedis.ttl(key) == -1) {
                shardedJedis.expire(key, expire);
            }
        }

        return 0;
    }
	
	/**
     * 删除锁
     *
     * @param key
     */
    public  void unlock(String key) {
        ShardedJedis shardedJedis = null;
        try {
            shardedJedis = shardedJedisPool.getResource();
            shardedJedis.del(key);
        } catch (Throwable e) {
            throw new RuntimeException("unlock error", e);
        }
    }
    
```