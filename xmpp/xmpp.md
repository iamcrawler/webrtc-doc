## XMPP协议原理介绍

XMPP(可扩展消息处理现场协议)是基于可扩展标记语言(XML)的协议，它用于即时消息(IM) 以及在线现场探测。它在促进服务器之间的准即时操作。这个协议可能最终允许因特网用户向因特网上 的其他任何人发送即时消息，即使其操作系统和浏览器不同。

XMPP 的前身是 Jabber，一个开源形式组织产生的网络即时通信协议。XMPP 目前被 IETF 国际标准组 织完成了标准化工作。标准化的核心结果分为两部分;

 IETF 中，把 IM 协议划分为四种协议，即即时信息和出席协议(Instant Messaging and Presence Protocol, IMPP)、出席和即时信息协议(Presence and Instant Messaging Protocol, PRIM)、针对即 时信息和出席扩展的会话发起协议(Session Initiation Protocol for Instant Messaging and Presence Leveraging Extensions, SIMPLE)，以及可扩展的消息出席协议(XMPP)。最初研发 IMPP 也是 为了创建一种标准化的协议，但是今天，IMPP 已经发展成为基本协议单元，定义所有即时通信协议应该 支持的核心功能集。
XMPP 和 SIMPLE 两种协议是架构，有助于实现 IMPP 协议所描述的规范。PRIM 最初是基于即时通信 的协议，与 XMPP 和 SIMPLE 类似，但是己经不再使用

1. XMPP 协议是公开的，由 JSF 开源社区组织开发的。XMPP 协议并不属于任何的机构和个人，而是
属于整个社区，这一点从根本上保证了其开放性。
2. XMPP 协议具有良好的扩展性。在 XMPP 中，即时消息和到场信息都是基于 XML 的结构化信息， 这些信息以 XML 节(XML Stanza)的形式在通信实体间交换。XMPP 发挥了 XML 结构化数据的通用传输层 的作用，它将出席和上下文敏感信息嵌入到 XML 结构化数据中，从而使数据以极高的效率传送给最合适 的资源。基于 XML 建立起来的应用具有良好的语义完整性和扩展性。
3. 分布式的网络架构。XMPP 协议都是基于 Client/Server 架构，但是 XMPP 协议本身并没有这样的 限制。网络的架构和电子邮件十分相似，但没有结合任何特定的网络架构，适用范围非常广泛。
4.XMPP 具有很好的弹性。XMPP 除了可用在即时通信的应用程序，还能用在网络管理、内容供稿、 协同工具、档案共享、游戏、远端系统监控等。
5.安全性。XMPP 在 Client-to-Server 通信，和 Server-to-Server 通信中都使用 TLS (Transport Layer Security)协议作为通信通道的加密方法，保证通信的安全。任何 XMPP 服务器可以独立于公众 XMPP 网络(例如在企业内部网络中)，而使用 SASL 及 TLS 等技术更加增强了通信的安全性。如下图所 示:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728171209607.png)

6.1.2 XMPP 协议网络架构

XMPP 是一个典型的 C/S 架构，而不是像大多数即时通讯软件一样，使用 P2P 客户端到客户端的架 构，也就是说在大多数情况下，当两个客户端进行通讯时，他们的消息都是通过服务器传递的(也有例外， 例如在两个客户端传输文件时).采用这种架构，主要是为了简化客户端，将大多数工作放在服务器端进 行，这样，客户端的 工作就比较简单，而且，当增加功能时，多数是在服务器端进行.XMPP 服务的框 架结构如下图所示.XMPP 中定义了三个角色，XMPP 客户端，XMPP 服 务器、网关.通信能够在这三 者的任意两个之间双向发生.服务器同时承担了客户端信息记录、连接管理和信息的路由功能.网关承 担着与异构即时通信系统的互联 互通，异构系统可以包括 SMS(短信)、MSN、ICQ 等.基本的网络形式 是单客户端通过 TCP/IP 连接到单服务器，然后在之上传输 XML，工作原理 是:

1) 节点连接到服务器;
2) 服务器利用本地目录系统中的证书对其认证; 3) 节点指定目标地址，让服务器告知目标状态; 4) 服务器查找、连接并进行相互认证;
5) 节点之间进行交互.

## XMPP 客户端
XMPP 系统的一个设计标准是必须支持简单的客户端。事实上,XMPP系统架构对客户端只有很少的几个限制。一个XMPP客户端必须支持的功能有

1) 通过TCP套接字与XMPP服务器进行通信; 
2) 解析组织好的 XML 信息包;
3) 理解消息数据类型。

XMPP 将复杂性从客户端转移到服务器端。这使得客户端编写变得非常容易，更新系统功能也同样变 得容易。XMPP 客户端与服务端通过 XML 在 TCP 套接字的 5222 端口进行通信，而不需要客户端之间直接 进行通信。

基本的 XMPP 客户端必须实现以下标准协议(XEP-0211):

1) RFC3920核心协议Core
2) RFC3921即时消息和出席协议InstantMessagingandPresence 3) XEP-0030服务发现ServiceDiscovery
4) XEP-0115实体能力EntityCapabilities

## XMPP 服务器

XMPP 服务器遵循两个主要法则:
5) 监听客户端连接，并直接与客户端应用程序通信; 
6) 与其他 XMPP 服务器通信;

XMPP 开源服务器一般被设计成模块化，由各个不同的代码包构成，这些代码包分别处理 Session 管 理、用户和服务器之间的通信、服务器之间的通信、DNS(Domain Name System)转换、存储用户的个人 信息和朋友名单、保留用户在下线时收到的信息、用户注册、用户的身份和权限认证、根据用户的要求 过滤信息和系统记录 等。另外，服务器可以通过附加服务来进行扩展，如完整的安全策略，允许服务器 组件的连接或客户端选择，通向其他消息系统的网关。

基本的 XMPP 服务器必须实现以下标准协议

1) RFC3920核心协议Core
2) RFC3921即时消息和出席协议InstantMessagingandPresence 3) XEP-0030服务发现ServiceDiscovery

## XMPP 网关
XMPP 突出的特点是可以和其他即时通信系统交换信息和用户在线状况。由于协议不同，XMPP 和其 他系统交换信息必须通过协议的转换来实现，目前几种主流即时通信协议都没有公开，所以 XMPP 服务器 本身并没有实现和其他协议的转换，但它的架构允许转换的实现。实现这个特殊功能的服务端在 XMPP 架 构里叫做网关(gateway)。目前，XMPP 实现了和 AIM、ICQ、IRC、MSN Massager、RSS0.9 和 Yahoo Massager 的协议转换。由于网关的存在，XMPP 架构事实上兼容所有其他即时通信网络，这无疑大大提 高了 XMPP 的灵活性和可扩展性。

## XMPP协议的组成

主要的 XMPP 协议范本及当今应用很广的 XMPP 扩展:
RFC 3920 :XMPP 核心。全称:The Extensible Messaging and Presence Protocol，即可 扩展通讯和表
示协议。说白了，就是规定基于 XML 流传输指定节点数据的协议。这么做的好处就是统一(注:大家都 按照这个定义，做的东西就可以相互通讯、交流，这个应该很有发展前景!)。它是一个开放并且可扩展 的协议，包括 Jingle 协议都是 XMPP 协议的扩展。(注:使用 Wireshark 抓包时，早期的版本可能找不到 这个协议，这时候可以选择 Jabber，它是 XMPP 协议的前身)。现在很多的 IM 都是基于 XMPP 协议开发 的，包括 gtalk 等。定义了 XMPP 协议框架下应用的网络架构，引入了 XML Stream(XML 流)与 XML Stanza(XML 节)，并规定 XMPP 协议在通信过程中使用的 XML 标签。使用 XML 标签从根本上说是协 议开放性与扩展性的需要。此外，在通信的安全方面，把 TLS 安全传输机制与 SASL 认证机制引入到内 核，与 XMPP 进行无缝的连接，为协议的安全性、可靠性奠定了基础。核心 文档还规定了错误的定义及 处理、XML 的使用规范、JID(Jabber Identifier，Jabber 标识符)的定义、命名规范等等。所以这是所有 基于 XMPP 协议的应用都必需支持的文档。
RFC 3921:用户成功登陆到服务器之后，发布更新自己的在线好友管理、发送即时聊天消息等业务。 所有的这些业务都是通过三种基本的 XML 节来完成的:IQ Stanza(IQ 节), Presence Stanza(Presence 节), Message Stanza(Message 节)。RFC3921 还对阻塞策略进行了定义，定义是多种阻塞方式。可以说， RFC3921 是 RFC3920 的充分补充。两个文档结合起来，就形成了一个基本的即时通信协议平台，在这个 平台上可以开发出各种各样的应用。
XEP-0030 服务搜索。一个强大的用来测定 XMPP 网络中的其它实体所支持特性的协议。
XEP-0115 实体性能。XEP-0030 的一个通过即时出席的定制，可以实时改变交变广告功能。
XEP-0045 多人聊天。一组定义参与和管理多用户聊天室的协议，类似于 Internet 的 Relay Chat，具有 很高的安全性。
XEP-0096 文件传输。定义了从一个 XMPP 实体到另一个的文件传输。
XEP-0124 HTTP 绑定。将 XMPP 绑定到 HTTP 而不是 TCP，主要用于不能够持久的维持与服务器
TCP 连接的设备。
XEP-0166 Jingle。规定了多媒体通信协商的整体架构。Jingle 协议是 XMPP 协议上的扩展协议，它着 手解决在 XMPP 协议框架下的点对点的连接问题，也即 P2P 连接。在 Jingle 框架下，即使用户在防火墙 或是 NAT 网络保护之下，也能够建立连接，从而提供文件传送、视频、音频服务等。
XEP-0167 Jingle Audio Content Description Format。定义了从一个 XMPP 实体到另一个的语音传输 过程。
TURN 协议:全称:Traversal Using Relays around NAT，顾名思义，就是通过中继服务器来传输数据 的协议。
STUN 协议:全称:Simple Traversal of UDP over NATs，即 NAT 的 UDP 简单穿越，它允许位于 NAT(或多重 NAT)后的客户端找出自己的公网地址，查出自己位于哪种类型的 NAT 之后以及 NAT 为 某一个本地端口所绑定的 Internet 端端口。知道 NAT 类型并且有了公网 IP 和 port，P2P 就方便多了。
XEP-0176 Jingle ICE(Interactive Connectivity Establishment)Transport。即 交互式连接建立，说 白了，它就是利用 STUN 和 TURN 等协议找到最适合的连接。
XEP-0177 Jingle Raw UDP Transport。纯 UDP 传输机制，文件讲述了如何在没有防火墙且在同一网 络下建立连接的。

XEP-0180 Jingle Video Content Description Format。定义了从一个 XMPP 实体到另一个的视频传输
过程。
XEP-0181 Jingle DTMF(Dual Tone Multi-Frequency)。 XEP-0183 Jingle Telepathy Transport Method。



