---
title:	网络层笔记
updated: 2017-12-09 14:40
---

1.ISP(Internet Service Provider)，互联网服务提供商，即向广大用户综合提供互联网接入业务、信息业务、和增值业务的电信运营商.

2.网络层的功能:屏蔽各种不同类型网络之间的差异，实现互连;了解通信子网的拓扑结构，选择路由，实现报文的网络传输

3.数据包在无连接的网路层服务中称为数据报(datagram).有连接的是虚电路。

4.面向连接服务的实现：连接表示符；
  标签交换：路由器需要替换出境数据包中的连接标识符

5.路由算法
  数据报：每一个包重新选择路径
  虚电路：建立电路的时候需要路由决策，也称为会话路由
  路由算法分为2类
  (1)非自适应算法:离线计算好，网络启动时下载到路由器中，也称为静态路由
  (2)自适应算法：动态计算

6.最优化原则：如果路由器 J 在路由器 I 到 K 的最优路由上，那么从J 到 K 的最优路径也遵循同样的路由。同最短路径

7.汇集树(sink tree):最短路径树。允许选择所有可能的路径则构成DAG.
DijKstra算法(宽带和延迟等实际测量作为距离，则距离非负)

8.路由算法本地技术:泛洪算法(flooding):把收到的每一个数据包，向除了该数据包到来的线路外的所有输出线路发送.改进：跳计数器，每一跳减一，为0则丢弃数据包；记录主机包序号，如果收到过则不转发。

9.距离矢量算法（动态路由算法）：也称为分布式Bellman-Ford路由算法，在Internet中对应协议名称为RIP协议。
  
  路由表：每个路由器为索引作为一个表项包含：目标路由器的出境路线和距离估计值。

  路由器向所有邻居结点发送它到每个目的结点的距离表，同时它也接收每个邻居结点发来的距离表，更新自己的路由表。

  无穷计数问题：对好消息反应迅速，对坏消息反应迟钝；改进水平分裂，不向邻居报告从邻居更新距离的表项。

10.链路状态路由算法：变种算法IS-IS或OSPF被Internet广泛使用

步骤：

  (1)发现邻居：发送HELLO特殊数据包,线路另一端路由应答.广播LAN引入人造节点N，LAN上指定一个路由运行路由协议即可。
	
  (2)设置链路成本距离：常用是带宽的反比或延迟。

  (3)构造链路状态包：标识符，Seq，Age
  
  (4)分发链路状态包：泛洪算法：序号小于已经看到过的数据报序号则丢弃。
	
		问题I:序号绕回，137年才能绕回，忽略
		
		问题II：路由器崩溃，序号记录表为0

		问题III：传输错误

		解决：age段，每秒年龄减一，年龄为0则丢弃

		改进：保留区等待
  (5)计算新路由：累计所有链路状态包后使用DijKstra算法

11.层次路由

  路由表过大，需要分层,即把路由器划分成区域。增加了路径长度，得不到最短路径。

12.移动主机路由
   
  转交地址：当前的地址
  
  所有主机都有家乡地址和位置

  家乡代理：主机告诉家乡位置的主机当前的转交地址
  (1)注册转交地址
  (2)往家乡地址发送数据包
  (3)隧道到转交地址
  (4)应答发送者
  (5)隧道到转交地址，绕过家乡位置

13.拥塞控制算法
  
  拥塞：网络中数据包过多导致延迟和丢失；网络层核传输层共同承担处理拥塞的责任。

  拥塞控制与流控制的差别：拥塞控制（congestion control）需要确保通信子网能够承载用户提交的数据流，是一个全局性问题，涉及主机、路由器等很多
	
  因素流控制（flow control）与点到点的传输有关，主要解决快速发送方与慢速接收方的问题，是局部问题，一般都是基于反馈进行控制的
  
  拥塞控制的途径：
    
   I.增加资源：购买更多宽带；流量感知路由
   
   II.减少负载：在虚电路网络中拒绝新连接，即准入控制

14.流量感知路由
	
  直接方式：权重相同情况下选择轻负载路径；导致路由振荡（最短路不断交替变化）问题。

  流量工程：不依赖与负载调整路由，由路由协议外部通过改变输入来调整路由。

15.流量调节：发送方调整传输速度
  
  拥塞参数：利用率（没有考虑实际冲突）、缓冲区排队数据包、丢包率（太晚）

  反馈机制：通知造成拥塞的发送方

  I.抑制包:目标地址为源主机，添加标记位不产生更多的抑制包
 
  II.显示拥塞通知：路由器在转发的包打标记表示自己正在经历拥塞，接收方应答时顺便告诉发送方
  
  III.逐跳后压：每一路由器收到抑制包则减慢发送给拥塞路由器的数据流。代价是上游路由器需要更大的缓冲区空间

16.负载脱落
 
  路由器来不及处理数据包时丢弃数据包。
  
  wine策略：旧的比新的好，文件传输
 
  mike策略：新的比旧的好，多媒体服务

17.流量整形：调节进入网络的数据流平均速率和突发性采用的技术

  漏桶和令牌桶区别：有漏洞和取出内容

18.网络互联

  多协议路由器：处理多个网络协议的路由器

  隧道：源主机和目标主机网络类型相同，中间隔着不同类型的网络。

19.互联网路由

  域内协议或内部网关协议:RIP,OSPF
  
  域间协议或外部网关协议：Internet上的是边界网关协议(BGP,Border Gaterway Protocol)
 
  AS(Autonomous System)自治系统：独立于其他网络运营，如ISP网络。一个ISP可能由多个AP组成

20.数据包长度
  
  路径最大传输单元(MTU)

  以太网：1500字节
  
  802.11协议：2272字节

  IP协议：65515字节

  透明分段：后续网络不关心是否分段，需要计数字段或者结束标识

  非透明分段：中间路由器不重新组合分段，当做原始数据包发送。IP给每个段数据包序号、字节偏移量和末尾标示位。

  现代Internet使用的是路径MTU发现。接受数据包太大路由器应答报错数据包让源主机重发数据包。

21.Internet的网络层

 IP(Internet Protocol):理论上数据包可容纳64KB.一般不超过1500字节(刚好放入以太网中)
 
22.IPv4

 头：20字节定长+可选变长(最长40字节)部分组成；IHL：IP分组头长度，最小为5，最大为15，单位为32bits.

23.IPv4地址

 IP地址不真正指向一台主机，而是一个网络接口。如果一台主机在两个网络上，则必须有两个IP地址。路由器有多个IP地址

 网络地址：前缀IP地址后接斜线，斜线后是网络部分地长度。"/16"读作"slash 16".前缀长度由子网掩码表示。

 无类域间路由（CIDR）：IP地址包含大小不等的前缀；最长匹配前缀
 
 分类寻址：与CIDR不一样，地址块大小固定。
 
 
 特殊地址：全0：表示本网络或本主机；全1：表示广播地址

24.IPV6

 16字节长书写：八组，每组4个16进制数

25.Internet控制协议

 ICMP控制消息协议：向数据包源端报告有关事件

 ARP协议：解决网络层地址（IP地址）与数据链路层地址（MAC地址）的映射问题
 
 I.建立一个ARP表，表中存放(IP地址，MAC地址)对

 II.若目的主机在同一子网内，用目的IP地址在ARP表中查找，否则用默认网关（通往网络外的路由器接口）的IP地址在ARP表中查找。

 III.若未找到，则发送广播分组，目的主机收到后给出应答，ARP表增加一项

 IIII.每个主机启动时，广播它的(IP地址，MAC地址)映射ARP表中的表项有生存期，超时则删除


 DHCP动态主机配置协议：DHCP服务器动态租借IP地址给主机
 
 


 



  
  






	





	



    




