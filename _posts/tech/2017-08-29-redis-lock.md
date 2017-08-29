---  
layout: post  
title: Redis实现分布式锁  
category: 技术  
tags: java、Redis  
keywords: java、Redis  
--- 

实现原理很多大神已经说的很清楚了，话不多说直接上源码

## 1. Redis命令介绍
### 1.1 [SETNX](http://redisdoc.com/string/setnx.html)
**SETNX key value**<br>
将 key 的值设为 value ，当且仅当 key 不存在。<br>
若给定的 key 已经存在，则 SETNX 不做任何动作。<br>
SETNX 是『SET if Not eXists』(如果不存在，则 SET)的简写。<br>
时间复杂度：<br>
O(1)<br>
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
    /**
	 *  获取锁
	 *
	 * @param key  锁的
	 * @param timeout	获取锁的等待时间，单位： 毫秒
	 * @param expire	获取锁后设置锁的有效时间，单位：秒
	 * @return
	 */
	public boolean lock(String key, long timeout, int expire) {
		boolean lock = false;
		ShardedJedis shardedJedis = null;

		try {
			shardedJedis = shardedJedisPool.getResource();
			//系统计时器的当前值，以毫微秒为单位。
			long nanoTime = System.nanoTime();
			//在timeout的时间范围内不断轮询锁
			while ((System.nanoTime() - nanoTime) < timeout) {
				//锁不存在的话，设置锁并设置锁过期时间，即加锁
				if (shardedJedis.setnx(key, key) == 1) {
					//设置锁过期时间是为了在没有释放,锁的情况下锁过期后消失，不会造成永久阻塞
					shardedJedis.expire(key, expire);
					//
					lock = true;
					return lock;
				}
				if (timeout <= 0L) {
					//没有设置超时时间，直接退出等待
					break;
				}
				logger.info("  >>>>>  出现锁等待 <<<<<<< ");
				//短暂休眠，避免可能的活锁
				Thread.sleep(300);
			}
		} catch (Exception e) {
			throw new RuntimeException("locking error", e);
		}
		return lock;
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
            shardedJedis.del(key);//直接删除
        } catch (Throwable e) {
            throw new RuntimeException("unlock error", e);
        }
    }