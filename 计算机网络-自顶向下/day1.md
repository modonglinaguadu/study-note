# 术语概念

> 分组 

当一台端系统向另一台端系统发送数据时，发送端把数据分段，并在每段添加首部字节

> 分组交换机

最著名的两类分组交换机：**路由器**和**链路层交换机**

> ISP

互联网提供商(Internet Service Provider)

> 边缘路由(edge router)

端系统到另一台远程端系统路径上的第一台路由器

> 接入网(access network)

端系统到边缘路由的物理链路

#### 宽带住宅接入有两种最流行的类型: 数字用户线路(DSL)和电缆(HFC)

> 数字用户线路(DSL)

Digital Subscriber Line

> 电缆因特网接入

住宅从提供有线电视的公司获得电缆因特网接入

特点：
    共享广播媒体，就是公用公共的下行信道和上行信道，当用的人多是，速度会慢

> 混合光纤同轴 HFC (Hybrid Fiber Coax)

它是电缆因特网接入的一种系统，光缆把电缆头连接到地区枢纽，然后使用传统的同轴电缆连接到各家各户



#### 光纤到户 FTTH(Fiber To The Home)

> 有源光网络 AON (Active Optical Network)

地区枢纽(本地交换局)到用户之间全部和部分采用光纤传输的通信系统

> 无源光网络 PON (Passive Optical Network)

![PON](https://github.com/modonglinaguadu/study-note/blob/master/image/network/PON.png)

用户 1--光纤--1 ONT\(光网络设备\)\[一种用户端的光网络终端\] n--光纤--1 光纤分配器  1--光纤--1 \< 中心局 OLT\(光纤线路终端\)\[提供光信号和电信号之间的转换\] \> ---- 因特网

