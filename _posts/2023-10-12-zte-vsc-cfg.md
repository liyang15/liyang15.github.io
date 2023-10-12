---
layout: post
title:  "zte VSC系统配置"
categories: netdevops
tags: Network Engineer
date: 2023-10-12
---

**VSC系统配置**

VSC系统简介

VSC是一种网络虚拟化技术，就是把多个可以单独运行的物理设备组合成一个虚拟设备，这些单独的物理设备通过堆叠单板链接在一起，彼此之间通过拓扑发现协议发现对方，并通过一定的机制选择一台主设备来管理整个VSC系统，其它设备作为备份设备担任转发角色。

通常情况下设备角色选举机制会将成员编号最小的设备选举为主设备。在主设备down掉或VSC系统两设备之间链路down掉的情况下，备份设备迅速接管VSC系统，保证系统运行及流量转发不受影响。

ZXR10 8900E支持用户对设备进行VSC相关配置，建立堆叠系统。VSC系统配置项包括：运行模式、成员编号、VSC域、堆叠端口、MAD相关设置等。同时ZXR10 8900E还支持对堆叠系统配置信息和运行信息的查看。

**VSC系统相关术语介绍：**

1. 

   运行模式：指设备工作在独立模式还是VSC模式。

   - 

     独立模式：处于该模式下的设备只能单机运行，不能与其他设备形成VSC。

   - 

     VSC模式：处于该模式下的设备可以与其他设备互连形成VSC。

2. 

   角色：指设备工作在VSC模式，按照功能不同分为Master、Slave、Standby。

   - 

     Master：负责管理整个VSC系统。

   - 

     Slave：作为Master的备份设备运行。

   - 

     Standby：作为待命主控运行。当Slave发生故障时，由Standby升为新的Slave。

   当Master故障时，系统会自动从Slave中选举一个新的Master接替原Master的工作。一个VSC中只能存在一台Master，一块Slave主控及最多两块Standby主控。

3. 

   VSC域：域是一个逻辑概念，设备通过VSC链路连接在一起就组成一个VSC，这些成员设备的集合就是一个VSC域。一台设备只能属于一个VSC域。

4. 

   成员编号：VSC中使用成员编号（Member ID）来标识和管理成员设备，VSC中所有设备的成员编号都是唯一的。

5. 

   堆叠口：在把两台或两台以上的设备组成VSC系统时，用来作为设备间通信的端口，堆叠口是类似SmartGroup的逻辑端口，由10GE/100GE口聚合而成。

6. 

   堆叠板：带内堆叠口所在的线卡称为堆叠板。每个机框最多支持两块堆叠板。每个堆叠板上最多可以配置8个堆叠口。目前可以用作堆叠板的线卡有：用作堆叠板的线卡有H1XF8A、H1XF16A、H1XF32A、H2XF8D、S1XF12A、H2XF48C、H4XF48S、H4XF32S、H4XF16S、H5XF16S、H5XF8S、H5XF32G48S、H1GF28C12X4C、H1GF28C12X2C、H1XF2B、H1XF4B、H1GT28C12X4B、H1GT28C12X2B、H2UC4C、H2UC2C、H2XF16C、H4UC4S、H4UC2S、H5GF48X8S、H1GF28C12X4S、H1XF32A、H1XF16A、H1XF8A、H1XF4A、H3XF12J、H3XF8J、H2XF48CM、H5GF16L2U2C、H2XF48C、H2XF48CM、H5GF16X32C、H5GF40X8C、H5XF16C和H2XF8C。

7. 

   VSC合并：两个独立的VSC系统（通常是两个单机VSC系统）各自稳定运行，通过物理连接和配置，发现对方后合并成一个VSC系统，这个过程称为VSC合并。

8. 

   VSC分裂：一个VSC系统形成后，如果两台设备堆叠口之间链路故障，导致VSC中两相邻成员设备物理上不连通，VSC系统会分裂成两个独立的VSC系统，这个过程称为VSC分裂。

9. 

   MAD检测：VSC分裂之后的两个VSC系统拥有相同的IP地址等三层配置，会引起地址冲突，导致故障在网络中扩大。

   为了提高系统的可用性，当VSC分裂时就需要一种机制，能够检测出网络中同时存在多个VSC，并进行相应的处理来降低VSC分裂对业务的影响。

   MAD检测即是这样一种机制，负责冲突检测、处理、恢复，但并不负责链路故障的检测。

配置VSC

VSC配置信息分两部分保存。一部分为基本配置信息，包括运行模式、成员编号，保存在NVRAM中，在系统上电之后可以读取。其他配置信息保存在Flash中，保存的文件路径是/flash/vscm/vscmCfg.dat。

ZXR10 8900E中提供了以下命令来进行VSC系统配置：

| 命令                                                         | 参数                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ZXR10(config-vsc)#vsc mode {**vsc**\| **alone**}[**shelfid**] | 配置设备下次启动后的VSC运行模式vsc \|alone：运行模式，必选alone：独立模式，处于该模式下的设备只能单机运行，不能与别的设备形成VSCvsc：VSC模式，处于该模式下的设备可以与别的设备互连形成VSCshelfid：当前所配置的VSC设备成员的机架号，可选项，不设置此参数时，默认配置当前机架 |
| ZXR10(config-vsc)#vsc memberid <memberid> [**shelfid**]      | 配置设备下次启动后加入VSC时的memberidmemberid：设备加入VSC系统的成员号，取值范围0~1，必选shelfid：当前所配置的VSC设备成员的机架号，可选项，不设置此参数时，默认配置当前机架 |
| ZXR10(config-vsc)#vsc domain <domainid> [**shelfid**]        | 配置下次启动后的VSC域编号domainid：VSC域编号，取值范围1~255，必选shelfid：当前所配置的VSC设备成员的机架号，可选项，设置此参数时，默认配置当前机架 |
| ZXR10(config-vsc)#vsc port-group 1 {**add**\| **remove** }[**hgoe**] <interface> | 添加或删除VSC端口（必须是支持higig模式设置的物理端口），堆叠模式下，当前生效。非堆叠模式，下次启动后生效1：vsc group编号，目前只支持一个vsc group，必选add\|remove：堆叠端口添加或删除选项，必选hgoe：为可选模式，普通堆叠口与hgoe口不能在同一个组中interface：待加入或待移除的vsc group的堆叠端口名，必选 |
| ZXR10(config-vsc)#vsc port-group 1 remove [**hgoe**] <1-12> **0** \|**1** | 清除某一块堆叠卡上的vsc group 1中的所有vsc端口。<1-12>：堆叠卡的槽位号0\|1：堆叠卡所在的Shelf号 |
| ZXR10(config-vsc)#vsc mad-port 1<interface> \|**clear**}     | 配置mad端口1：mad-port编号，必选< interface>：mad端口，必选clear：清除mad冲突接口 |
| ZXR10(config-vsc)#vsc write [**shelfid**]                    | 在执行任何VSC配置命令之后，需要执行此命令将用户配置保存起来，否则配置下次启动后不生效shelfid：当前所配置的VSC设备成员的机架号，可选项，不设置此参数时，默认配置当前机架 |

 说明：

VSC配置之后需通过vsc write命令保存配置，除了vsc port_group和vsc mad_port之外的配置命令，重启设备之后配置命令才会生效。vsc port_group和vsc mad_port这两条命令在堆叠模式下实时生效的。

show vsc config命令显示的是当前生效的VSC配置命令。

show vsc next命令显示的是当前配置保存的VSC配置命令，重启之后生效。

维护VSC

ZXR10 8900E使用show命令来查看VSC系统相关信息，常见的查看命令如下:

| 命令                                | 参数                                                  |
| ----------------------------------- | ----------------------------------------------------- |
| ZXR10#show vsc config [**shelfid**] | 显示VSC系统当前配置                                   |
| ZXR10#show vsc next [**shelfid**]   | 显示VSC系统的下一次配置，这些配置在下一次重启后生效   |
| ZXR10#show vsc information          | 显示VSC系统当前的运行信息，如主备、链路状态和连接时长 |
| ZXR10#show vsc link [**shelfid**]   | 显示VSC系统当前的带内堆叠链路状态信息                 |
| ZXR10#show vsc port                 | 显示带外堆叠口状态                                    |

参数解释如下：

| 参数        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| **shelfid** | 当前所配置的VSC设备成员的机架号，可选项，不设置此参数时，默认显示当前机架信息 |

举例

1. 

   显示本机框当前VSC配置：

   ```
   ZXR10#show vsc config
    Mode:               cluster                                                                                                        
    Memberid:           0                                                                                                              
    Domain:             15                                                                                                             
    Mad port:           xgei-0/6/0/1                                                                                                   
    Mad stat:           Normal                                                                                                         
    Vsc group number:   1                                                                                                              
                                                                                                                                       
    Groupid:            1                                                                                                              
    Group name:         vsc group 1                                                                                                    
    Port status:        Normal                                                                                                         
    Port number:        2                                                                                                             
    Port list:                                                                                                                         
    Shelf     Panel     Port                                                                                                           
    -----------------------                                                                                                            
    0         4         25                                                                                                             
    0         4         26       
   ```

2. 

   显示本机框当前VSC系统运行信息：

   ```
   ZXR10#show vsc information
   Vsc domain       : 1
   Vsc band mode   : IN
   Master MP        : 0
   Master shelf type: ZXR10 8908E  
   Slave MP         : 1
   Slave shelf type : ZXR10 8908E  
   Member MP        : NONE         
   Vsc setup time   : 2016/09/23-01:35:21
   Vsc running time : 0d/0h/26m/52s
   System cur-time  : 2016/09/23-02:02:13
   ```

3. 

   显示本机框当前VSC的在下一次重启时生效的配置信息：

   ```
   ZXR10#show vsc next
     Mode:               cluster                                                                                                        
    Memberid:           0                                                                                                              
    Domain:             15                                                                                                             
    Mad port:           xgei-0/6/0/1                                                                                                   
    Mad stat:           Normal                                                                                                         
    Vsc group number:   1                                                                                                              
                                                                                                                                       
    Groupid:            1                                                                                                              
    Group name:         vsc group 1                                                                                                    
    Port status:        Normal                                                                                                         
    Port number:        2                                                                                                             
    Port list:                                                                                                                         
    Shelf     Panel     Port                                                                                                           
    -----------------------                                                                                                            
    0         4         25                                                                                                             
    0         4         26                                        
   ```

4. 

   显示本机框当前VSC的带内堆叠链路信息：

   ```
   ZXR10#show vsc link
   ========================================================================
   Local                : local port of this link,show as group(shelf,slot,port)
   Remote               : remote port of this link,show as group(shelf,slot,port) 
   M/S                  : link is master or standby                             
   Port                 : the local port state                                  
   Learn                : the remote port learn state                           
   LinkNow              : current state of this link                            
   RecordTime           : the time of this link connected or interrupted        
   ========================================================================
   Group 1 Link Information
   Local          Remote         M/S  Port  Learn    LinkNow  RecordTime
   ========================================================================
    1( 0, 5, 8)   1( 1, 5, 2)    M    UP    ACTIVE   UP       2016/09/23-09:41:05
   ```

5. 

   显示本机框当前V带外堆叠口状态。

   ```
   ZXR10#show vsc port
   Shelf Panel Port    PhyState DetectState
   1     T1    vsc1    DOWN     NOSTART
   1     T1    vsc2    DOWN     NOSTART
   1     T2    vsc1    DOWN     NOSTART
   1     T2    vsc2    DOWN     NOSTART
   ```

VSC配置实例

配置说明

ZXR10 8900E支持形成VSC系统的机框型号为8905E、8908E和8912E，而8902E不支持堆叠。堆叠时使用两个相同型号的机框进行堆叠。

每个机框最少需要一块主控板，否则这个设备不能加入VSC。如果加入VSC的某个设备的所有主控都失效了，则认为这个设备退出了VSC。

每个机框中需要最少一块最多两块堆叠板，在堆叠板上配置堆叠端口互连，使VSC系统中的设备相互连通。堆叠板要求其物理口要能工作在higig模式，可以作为堆叠板的类型有8×10GE单板和12×10GE单板。

配置实例如下：

1. 

   配置设备下次启动后的工作模式为VSC：

   ```
   ZXR10(config)#vsc
   ZXR10(config-vsc)#vsc mode vsc
   ZXR10(config-vsc)#vsc write
   ```

2. 

   配置设备下次启动后加入VSC时的memberid：

   ```
   ZXR10(config-vsc)#vsc memberid 0
   ZXR10(config-vsc)#vsc write
   ```

3. 

   配置设备下次启动后加入VSC时的domain：

   ```
   ZXR10(config)#vsc
   ZXR10(config-vsc)#vsc domain 20
   ZXR10(config-vsc)#vsc write
   ```

4. 

   添加堆叠端口：

   ```
   ZXR10(config)#vsc
   ZXR10(config-vsc)#vsc port-group 1 add  xgei-0/7/0/1  
   ZXR10(config-vsc)#vsc port-group 1 add  xgei-0/7/0/2  
   ZXR10(config-vsc)#vsc write
   ```

5. 

   配置MAD端口：

   ```
   ZXR10(config)#vsc 
   ZXR10(config-vsc)#vsc mad-port 1 xgei-0/2/0/1
   ZXR10(config-vsc)#vsc write
   ```