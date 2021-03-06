﻿## 新生命Redis组件
`NewLife.Redis` 是一个Redis客户端组件，以高性能处理大数据实时计算为目标。  
Redis协议基础实现Redis/RedisClient位于[X组件](https://github.com/NewLifeX/X)，本库为扩展实现，主要增加列表结构、哈希结构、队列等高级功能。  

源码： https://github.com/NewLifeX/NewLife.Redis  
Nuget：NewLife.Redis  

---
### 特性
* 在ZTO大数据实时计算广泛应用，200多个Redis实例稳定工作一年多，每天处理近1亿包裹数据，日均调用量80亿次
* 低延迟，Get/Set操作平均耗时200~600us（含往返网络通信）
* 大吞吐，自带连接池，最大支持1000并发 
* 高性能，支持二进制序列化

---
### Redis经验分享
* 在Linux上多实例部署，实例个数等于处理器个数，各实例最大内存直接为本机物理内存，避免单个实例内存撑爆
* 把海量数据（10亿+）根据key哈希（Crc16/Crc32）存放在多个实例上，读写性能成倍增长 
* 采用二进制序列化，而非常见Json序列化 
* 合理设计每一对Key的Value大小，包括但不限于使用批量获取，原则是让每次网络包控制在1.4k字节附近，减少通信次数
* Redis客户端的Get/Set操作平均耗时200~600us（含往返网络通信），以此为参考评估网络环境和Redis客户端组件
* 使用管道Pipeline合并一批命令
* Redis的主要性能瓶颈是序列化、网络带宽和内存大小，滥用时处理器也会达到瓶颈
* 其它可查优化技巧
以上经验，源自于300多个实例4T以上空间一年多稳定工作的经验，并按照重要程度排了先后顺序，可根据场景需要酌情采用！
