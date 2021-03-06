### 什么是IOTC?

IOTC全称为Internet of Things Cloud，翻译为物联网云

### TUTK IOTC平台基本概念

基本概念包含五个部分：

1. IOTC平台使用的技术说明；

2. IOTC平台的4个关键参与者以及它们的职责；

3. 详细描述各部分协作，提供IOTC平台的概述；

4. 简要描述了IOTC平台中的三个重要的数据通信模块极其关系

5. IOTC平台的渗透能力

#### 专业术语

* IOTC Platform — 物联网云平台，TUTK提供的在不同设备客户端之间通过简单的配置建立P2P连接，传输不同类型数据的解决方案。

* Master — 由TUTK在IOTC平台中维护的机器，用于管理服务器和设备，以及通过检查内部密钥信息来验证它们

* Server - 由TUTK或设备制造商在IOTC平台中维护的机器，用于通过P2P或中继服务来处理设备和客户端之间的连接

* Devices - 由供应商制造的设备，be capable of IOTC platform 并且即使放在NAT之后，也能与客户端（Client）建立连接

* Client - 连接Devices的终端，在设备间传输数据

* UID - 由TUTK为每个服务器和设备发出的二十字节的唯一标识，用于管理和连接的作用

* VPG - 代替供应商的、产品和组ID的一组ID，是生成UID的一部分因素

* IOTC Module - 在Devices和Client之间，提供基本的数据传输的一种数据传输模型

* IOTC Session - 在Device和Client之间建立的连接，一个Device可以有多个设备和多个Session，反之亦然

* IOTC Channel - 在一个Session中的通信路径，用于在Device和Client之间读写数据，同时一个Session可以多个Channel同时传输数据

* RDT Module - 在IOTC平台里的一种数据传输模型，在RDT（Reliable Data Transfer ）服务器和RDT客户端之间一双向的方式提供可靠的数据传输

* RDT Server - 使用RDT和RDT Client传输和接收数据的Device

* RDT Client - 使用RDT和RDT Server传输和接收数据的Client

* RDT Channel - 基于IOTC Channel，在RDT Client和RDT Server之间建立的Channel,一个RDT Server可以有一个RDT Client和多个RDT Channels

* AV Model - IOTC平台上提供的一种数据传输模型，从AV Server和AV clients单向获取流畅的音/视频流

* AV Server - 将音视频传送到相应的AV Client的device或者client

* AV Client - 从相应的AV Server获取音视频的device或者client

* AV Channel - 给予IOTC Channel,在AV Server和 AV Client之间简历的Channel.一个AV Server可以有一个AV Client和多个AV Channel

* AV IO Control - 一组指令，比如播放、停止等等，用在device制造商定义的AV通信模型中，用于AV Server和AV Client之间的通信

* Tunnel server - 即使设备放在在NAT之后也能被Tunnel agents连接到的使用P2PTunnel模型的设备

* Tunnel agent - 连接到Tunnel Server,用于P2P通讯的P2PTunnel的设备

* AES - IOTC平台支持的一种工业加密方式，保护devices和clients之间的数据传输

#### 角色和职责

在IOTC平台中，master、server、device和client四个主要参与者相互协作完成P2P或者通过在device和client上通过简单的配置完成中继服务。以下是它们在IOTC平台中的主要职责：

* Master * 验证Server和Client的UID * 管理连接的server * 向设备和客户端提供与特定VPG匹配的服务器列表

* Server 
  * 管理登录的client 
  * 提供客户端P2P或中继服务与设备通信 

* Device 
  * 登录到与特定VPG匹配的服务器 
  * 监听来自Client的连接 * 和clients通信 

* Client 
  * 请求master提供与device特定的VPG匹配的服务器列表 
  * 向server请求帮助与devices建立连接 
  * 和devices通信 
* Tunnel Server 
  * 通过UID登录severs 
  * 监听来自Tunnel agent的连接请求 
  * 和Tunnel agent通信 
* Tunnel Agent 
  * 凭借UID请求master提供servers的列表 
  * 请求Servers与Tunnel Server建立P2P连接
  * 和Tunnel Server通信 

#### 协作

下图描绘了master, server, device和client 在IOTC平台中的协作关系：![IOTC各参与者协作关系图](/assets/01-IOTC协作关系图.png)

在上图中定义了六条路径，并且它们在对等体之间的协作行为描述如下。注意以下是典型、通用的协同工作行为。 为了便于说明，暂时省略了错误处理或来回通信的细节。

* Path 1 
  * 当一个Server启动，通过提供自身的UID链接到所选的masters 
  * Masters验证server的UID是否有效。如果server能够被masters识别，服务器信息，比如IP地址和VPG，将会存储在masters里面 
* Path 2 
  * 当一个device想加入IOTC平台，需要凭借其UID链接到所选的master 
  * Master会检测device的UID是否有效，如果有效，master会向device反馈一个可连接的并且已将PVG和device匹配的server列表

* Path 3 
  * 一旦device从master获取到了可连接的servers的列表，它会通过UID登录到这些servers 
  * severs验证device的UID是否有效，server将保存device的信息，并回应设备登录程序完成 
  * 一旦device成功登录servers,device开始监听来自client的所有的连接 
* Path 4 
  * 当Client想使用IOTC平台的服务，它会连接到所选的服务器并凭借UID询问管理device的servers列表 
  * master返回servers列表到client 

* Path 5 
  * 一旦client从master获取到servers的列表，它会要求哪些服务器和指定的device建立连接 
  * servers基于网络状态，在device和client之间创建P2P或者中继服务

* Path 6  
  * device和client之间的连接已经准备完毕，可以相互传输数据了 
  * 三种通讯模型可以用于三种不同的目的：IOTC Model、RDT Mode、AV Model 

#### 数据传输模型
IOTC平台提供了三中数据通讯模型 
* IOTC Model 
* RDT Mode 
* AV Model 

通过设计，RDT模块和AV模块利用来自IOTC模块的服务，同时提供更具体的数据传输方式。 他们的关系可以用下图描述：
![IOTC提供的三种数据传输模型](/assets/02-IOTC平台提供的三中数据传输模型.png)

在RDT模型中，数据会以可靠的方式传输，以确保所有的数据都被接收到。另一方面，AV Model以管理的方式传输，根据网络状态音频数据尝试和帧数据保持同步、保证流畅。

因为三种传输数据的方式的目的不同，他们的好处和开销也各不相同。因此建议调研在client和device中开发什么类型的功能，然后从三种传输模型中选择最合适的一种。

#### IOTC P2P Tunnel Module

Tunnel连接通过公共网络上提供了一个私有的传输路径。下面描述了IOTC P2PTunnel模型支持的协议和示意图表：
![IOTC P2P Tunnel Model](/assets/03-IOTC P2P Tunnel Model.png)


#### 穿透能力