###  TTCP协议

#### 一、作用

测试TCP连接之间的吞吐量

#### 考虑因素

+ 满足CPU和带宽充分运用

+ 吞吐量：每秒发送消息数
+ 额外负载

#### 二、为什么实现TTCP

+ 使用了基本的`Sockets APIS`：socket,listen,bind,accept,connect,read/recv,write/send,shutdown,close等
+ 协议是二进制的，不仅仅是字节流，所以它比传统的`echo`样例更好
+ 能够体现语言本身的速度
+ 不是并发的，比较基础

#### 三、协议介绍

响应接收模型



