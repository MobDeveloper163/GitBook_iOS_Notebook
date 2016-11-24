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

* Server * 管理登录的client * 提供客户端P2P或中继服务与设备通信 * Device * 登录到与特定VPG匹配的服务器 * 监听来自Client的连接 * 和clients通信 * Client * 请求master提供与device特定的VPG匹配的服务器列表 * 向server请求帮助与devices建立连接 * 和devices通信 * Tunnel Server * 通过UID登录severs * 监听来自Tunnel agent的连接请求 * 和Tunnel agent通信 * Tunnel Agent * 凭借UID请求master提供servers的列表 * 请求Servers与Tunnel Server建立P2P连接 * 和Tunnel Server通信 #### 协作

下图描绘了master, server, device和client 在IOTC平台中的协作关系：![IOTC各参与者协作关系图](/assets/01-IOTC协作关系图.png)


