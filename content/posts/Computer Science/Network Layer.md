---
title: "计算机网络 网络层"
date: 2015-07-05T08:57:52+10:00
draft: true
categories: ["Computer Science"]
---

# 计算机网络

## 网络层

### 地址解析协议 ARP

- 网络层实现主机之间的通信，链路层实现每段链路之间的通信；在通信过程中，IP 数据报的源地址和目的地址始终不变，而 MAC 地址随着链路的改变而改变
- ARP 协议从 IP 地址获取 MAC 地址
- 每个主机有一个 ARP 高速缓存，里面有本局域网上的各主机和路由器的 IP 地址到 MAC 地址的映射表
- 如果主机 A 知道主机 B 的 IP 地址，但是 ARP 高速缓存中没有该 IP 地址到 MAC 地址的映射，此时主机 A 通过广播的方式发送 ARP 请求分组，主机 B 收到该请求后会发送 ARP 响应分组给主机 A 告知其 MAC 地址，随后主机 A 向其高速缓存中写入主机 B 的 IP 地址到 MAC 地址的映射

### 网际控制报文协议 ICMP

- ICMP 是为了更有效地转发 IP 数据报和提高交付成功的机会。它封装在 IP 数据报中，但是不属于高层协议
- ICMP 报文分为差错报告报文和询问报文

#### 应用

- 1. Ping
    - Ping 是 ICMP 的一个重要应用，主要用来测试两台主机之间的连通性
    - Ping 的原理是通过向目的主机发送 ICMP Echo 请求报文，目的主机收到之后会发送 Echo 回答报文；Ping 会根据时间和成功响应的次数估算出数据包往返时间以及丢包率

- 2. Traceroute
    - Traceroute 是 ICMP 的另一个应用，用来跟踪一个分组从源点到终点的路径
    - Traceroute 发送的 IP 数据报封装的是无法交付的 UDP 用户数据报，并由目的主机发送终点不可达差错报告报文
    - 源主机向目的主机发送一连串的 IP 数据报。第一个数据报 P1 的生存时间 TTL 设置为 1，当 P1 到达路径上的第一个路由器 R1 时，R1 收下它并把 TTL 减 1，此时 TTL 等于 0，R1 就把 P1 丢弃，并向源主机发送一个 ICMP 时间超过差错报告报文
    - 源主机接着发送第二个数据报 P2，并把 TTL 设置为 2。P2 先到达 R1，R1 收下后把 TTL 减 1 再转发给 R2，R2 收下后也把 TTL 减 1，由于此时 TTL 等于 0，R2 就丢弃 P2，并向源主机发送一个 ICMP 时间超过差错报文
    - 不断执行这样的步骤，直到最后一个数据报刚刚到达目的主机，主机不转发数据报，也不把 TTL 值减 1。但是因为数据报封装的是无法交付的 UDP，因此目的主机要向源主机发送 ICMP 终点不可达差错报告报文
    - 之后源主机知道了到达目的主机所经过的路由器 IP 地址以及到达每个路由器的往返时间