---
layout: post
title:  "基本SmartGroup配置实例"
categories: netdevops
tags: Network Engineer
date: 2023-10-20
---

**基本SmartGroup配置实例**

配置说明

如[图1](http://127.0.0.1:8890/files/Lib20180125163910_R2_4/documents/链路层协议/topics/43-基本SmartGroup配置实例.html#SmartGroup配置实例-2FFF657E__ad模式配置-9C13ACB3)所示，S1和S2之间运行LACP协议。S1接口gei-0/2/0/5与S2接口gei-0/3/0/5直连，S1接口gei-0/2/0/9与S2接口gei-0/3/0/9直连。

S1===LACP===S2

图1 802.3ad模式配置

配置思路

1. S1上创建smartgroup1，S2上创建smartgroup1，并进入接口配置模式

2. 分别在S1，S2上的接口配置模式下设置smartgroup1接口的交换属性，并退出到全局模式

3. 全局模式下进入LACP配置模式，再进入所要配置的smartgroup接口

4. 将S1，S2上smartgroup1的聚合控制方式配置为采用802.3ad标准的LACP协议，配置负荷分担策略以及最小成员数

5. 全局模式下进入LACP配置模式，再进入所要配置实接口

6. 分别将图中S1，S2的实接口绑入smartgroup1

7. 分别将S1，S2上smartgroup1中成员接口配置LACP协商模式以及超时时长

配置过程

S1上的配置如下：

```
S1(config)#interface smartgroup1                                                                                                 
S1(config-if-smartgroup1)#exit 
S1(config)#lacp
S1(config-lacp)#interface smartgroup1
S1(config-lacp-sg-if)#lacp mode 802.3ad
S1(config-lacp-sg-if)#lacp load-balance  dst-mac
S1(config-lacp-sg-if)#lacp  minimum-member 1
S1(config-lacp-sg-if)#exit 
S1(config-lacp)#interface gei-0/2/0/5 
S1(config-lacp-member-if)#smartgroup 1 mode active
S1(config-lacp-member-if)#lacp timeout short
S1(config-lacp-member-if)#exit
S1(config-lacp)#interface gei-0/2/0/9 
S1(config-lacp-member-if)#smartgroup 1 mode active
S1(config-lacp-member-if)#lacp timeout short
S1(config-lacp-member-if)#exit
```

S2上的配置如下：

```
S2(config)#interface smartgroup1                                                                                                 
S2(config-if-smartgroup1)#exit 
S2(config)#lacp
S2(config-lacp)#interface smartgroup1
S2(config-lacp-sg-if)#lacp mode 802.3ad
S2(config-lacp-sg-if)#lacp load-balance  dst-mac
S2(config-lacp-sg-if)#lacp  minimum-member 1
S2(config-lacp-sg-if)#exit
S2(config-lacp)#interface gei-0/3/0/5 
S2(config-lacp-member-if)#smartgroup 1 mode active 
S2(config-lacp-member-if)#lacp timeout short 
S2(config-lacp-member-if)#exit
S2(config-lacp)#interface gei-0/3/0/9
S2(config-lacp-member-if)#smartgroup 1 mode active 
S2(config-lacp-member-if)#lacp timeout short
S2(config-lacp-member-if)#end
```

配置验证

查看S1的SmartGroup配置和生效情况：

```
S1(config)#show lacp 1 internal 
Smartgroup:1
Flags:            * - Port is Active member Port 
                  S - Port is requested in Slow LACPDUs   F - Port is requested  
in Fast LACPDUs 
                  A - Port is in Active mode             P - Port is in Passive
mode
Actor             Agg        LACPDUs  Port     Oper   Port  RX            Mux 
Port[Flags]       State      Interval Priority Key    State Machine   Machin 
e 
--------------------------------------------------------------------------------
gei-0/2/0/5 [FA*]  ACTIVE     1        32768    0x111  0x3f  CURRENT       COLL  
/*端口聚合。Active：聚合成功；Inactive：聚合失败*/ 
gei-0/2/0/9 [FA*]  ACTIVE     1        32768    0x111  0x3f  CURRENT       COLL

S1(config)#show running-config-interface smartgroup1 
!<if-intf> 
interface smartgroup1 
$ 
!</if-intf>
!<LACP>
lacp
    interface smartgroup1
     lacp load-balance  dst-mac
  $
$ 
!</LACP> 
lacp 
  interface smartgroup1 
    lacp mode 802.3ad                
/*协商模式*/ 
    lacp minimum-member 1            
/*聚合成功最小成员数，只有聚合成功的链路大于或等于最小成员数，SmartGroup协议才会up*/ 
  interface gei-0/2/0/9 
    smartgroup 1 mode active  
/*802.3ad模式下，链路的两端必须至少要有一个配置为active模式，该链路才会聚合成功 */
    lacp timeout short 
  interface gei-0/2/0/5 
    smartgroup 1 mode active 
    lacp timeout short 
! </LACP>     

S1(config)#show lacp 1 neighbors     /*查看邻居*/ 
Smartgroup 1  neighbors 
Actor         Partner              Partner   Port      Oper    Port 
Port          System ID            Port No.  Priority  Key     State
--------------------------------------------------------------------- 
gei-0/2/0/9   0x8000,00d0.d012.1127    21      0x8000    0x111   0x3f
gei-0/2/0/5   0x8000,00d0.d012.1127    17      0x8000    0x111   0x3f

S1(config)#show lacp 1 counters 
Smartgroup:1 
Actor         LACPDUs               Marker    LACPDUs    Marker 
Port          Tx         Rx         Tx  Rx    Err        Err 
------------------------------------------------------------------- 
gei-0/2/0/9   1840       1840       0   0     0          0           
/*依据配置的timeout参数，Tx和Rx的数值每30秒或1秒增加1*/
gei-0/2/0/5   1840       1840       0   0     0          0
```

