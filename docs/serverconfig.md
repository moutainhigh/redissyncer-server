# 培训详解
### appliction.yml参数及请求值

#### 1）.配置文件默认配置参数    

    syncerplus:
      redispool:
        #池中空闲链接回收线程执行间隔时间  例：每隔1000毫秒执行一次回收函数
        timeBetweenEvictionRunsMillis: 300000
        #池中空闲连接回收未使用的时间  例：1800000毫秒未使用则回收 
        idleTimeRunsMillis: 600000
        #最小池大小
        minPoolSize: 1
        #最大池大小
        maxPoolSize: 25
        #连接超时时间
        maxWaitTime: 2000
    
      poolconfig:
        #核心池大小
        corePoolSize: 100
        #最大池大小
        maxPoolSize: 1500
        #队列最大长度
        queueCapacity: 1600
        #线程池维护线程所允许的空闲时间
        keepAliveSeconds: 300

#### 2）. 开启同步任务 
    （maxPoolSize请设置小于redis-server允许连接的客户端数量）
    请求头：
    Content-Type:application/json;charset=utf-8;
    请求体：
    {
        "sourceUri": "redis://127.0.0.1:6379?authPassword=123456",
        "targetUri": "redis://127.0.0.1:6380?authPassword=123456",
        "threadName": "test01"
    }
    或（携带连接池参数）
    {
        "sourceUri": "redis://127.0.0.1:6379?authPassword=123456",
        "targetUri": "redis://127.0.0.1:6380?authPassword=123456",
        "threadName": "test01",
        "idleTimeRunsMillis": 300000,
        "maxPoolSize": 20,
        "maxWaitTime": 10000,
        "minPoolSize": 1
    }


|  参数   | 含义  |     缺省
    -|-|-
| sourceUri  | 源redis连接地址         |不可缺省|
| targetRedisUri  | 单元格             |不可缺省|
| sourceUri  | 源redis连接地址         |不可缺省|
| threadName  | 任务名称               |不可缺省|
| sourceUri  | 源redis连接地址         |不可缺省|
| minPoolSize  | redis池最小大小       | 可缺省 |
| maxPoolSize  | redis池最大小         | 可缺省 |
| maxWaitTime  | 超时时间              | 可缺省 |
| idleTimeRunsMillis  | 回收空闲未使用时间连接 | 可缺省|
    

    上述参数缺省时为数application.yml文件默认配置参数    

    返回结果为以下参数下时为启动成功：
    {
        "msg": "success",
        "code": "200"
    }
    
#### 3）. 查看正在执行的任务列表 
    获取正在运行同步任务列表(GET请求)
    
    /sync/listAlive
    
#### 4）. 查看已关闭的任务列表    
    获取已结束同步任务列表(GET请求)
    
    /sync/listDead
    
#### 5）.根据任务名称关闭同步任务
    根据任务名称关闭同步任务
    请求接口(DELETE请求)：
    
    /sync/closeSync/${threadName}
    
    参数：
        任务名称
        ${threadName}  ： AtoB
        
         /sync/closeSync/AtoB
         
         