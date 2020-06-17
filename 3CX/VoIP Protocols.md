# VoIP Protocols(VoIP 协议)

## Introduction（介绍）

多年以来，两种类型的网络之间一直存在明确的划分：

 Voice Network（语音网络），基于电路交换，保留了电路，并且通信期间的路由始终通过同一路径进行。示例：公用交换电话网（PSTN）

Data networks（数据网络）
基于分组交换，信息分为分组，每个分组可以通过不同的路径传播。示例：Internet
为了通过基于数据包交换的数据网络（Internet）发送信息，必须采用负责传输和恢复信息的协议。

The problem with the circuit switching technology is that it requires a great amount of bandwidth for each call
（电路交换技术的问题在于，每个呼叫都需要大量带宽）。该电路未得到有效使用，因为在通话过程中使用了相同的通道，并且大多数电话呼叫都具有许多静默时刻。

相反，数据网络仅在必要时才有效地使用带宽来传输信息。由于最终系统具有恢复原始信息的程序，因此延迟，数据包传递延迟的变化以及包裹的丢失不应成为不利因素。但是，语音和视频流对这些参数非常敏感。因此，需要具有高度QoS（服务质量）的网络和协议。

IP语音（VoIP）定义了必要的路由系统和协议，用于通过Internet传输语音对话。Internet是基于TCP / IP协议的数据包交换网络。

目前，主要有两种用于Internet语音传输的VoIP VoIP架构，它们非常常用：

- SIP Session Initiation Protocol（会话发起协议）
SIP是会话发起协议的缩写，是IETF开发的标准，标识为RFC 3261， 2002年SIP是用于在IP网络中建立呼叫和会议的信令协议。会话的开始，会话的更改或期限与呼叫中使用的应用程序类型无关。一个会话可以包含多种数据类型，包括音频，视频和许多其他格式

- H.323
H.323是通信多媒体的第一个国际标准，它促进了语音，视频和数据的融合。最初，人们想到分组电路网络，在分组电路网络中，他发现了自己与IP集成的实力，这是VoIP中非常常用的协议。


[下一章SIP Protocol](./SIP_Protocol.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[原文](http://www.en.voipforo.com/SIP/SIP_architecture.php)
