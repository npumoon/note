
RPC框架相关概念

## 服务治理

* 服务路由
  1. 权重：机器配置权重高低
  2. IP路由
  3. 分组路由
  4. 参数路由：根据方法名进行读写；
  5. 机房路由：例如，只走同机房、或者同机房优先。
  
* 调用授权
* 动态分组

* 调用限流

  从架构上来说，分为分布式限流 和 单机限流。
  从算法角度说，常用的令牌桶限流、漏桶限流、滑动时间窗口
  1. 服务端限流：
     1. 基于令牌桶
        
        （匀速向桶里放令牌，满时丢弃令牌，处理请求前，先取令牌，然后执行请求，若请求数大于当前存在令牌数，则阻塞等待）
         令牌产生的速度恒定，令牌消耗（获取多个令牌来处理请求）的速度可以不限
     2. 漏桶
     
        请求来了之后，放入桶内，然后请求漏出速度恒定，稳定请求处理速度。
        （突发的流量，经过漏桶后，整形为稳定的流量）

