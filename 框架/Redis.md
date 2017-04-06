[TOC]
# 代码配置集群事务

```java
package com.bjsxt.utils;

import java.util.Map;
import java.util.Map.Entry;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisCluster;
import redis.clients.jedis.JedisPool;

public class JedisUtil {

    /**
     * 根据Key获取redis节点
     */
    public static Jedis getJedisByKey(JedisCluster cluster,String key){
        Jedis jedis = null;
        //自定义逻辑，实现redis集群事务
        Map<String,JedisPool> poolMap = cluster.getClusterNodes();
        for(Entry<String,JedisPool> entry:poolMap.entrySet()){
            jedis = entry.getValue().getResource();
            try {
                boolean exists = jedis.exists(key);
                if(exists){
                    return jedis;
                }
            } catch (Exception e) {
                /*
                这里不能打印堆栈信息，机制是会从所有集群中的服务器来确认是否存在对应的key,有一个不存在就会报错，而我们只需要有一个存在即可
                */
            }
        }
        return jedis;
    }
}
```