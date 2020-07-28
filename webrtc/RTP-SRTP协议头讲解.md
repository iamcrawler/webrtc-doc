## 整个浏览器使用的协议图
左边为浏览器使用的传统协议栈
右边为WebRTC的协议栈
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728074350205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- SCTP流控传输协议
- TLS和DTLS都是用于传输证书和加密算法的协议
- TCP是基于流式的 UDP是基于数据报形式

## RTP/SRTP
两者的区别，实际上是多了一层安全，RTP是数据没有加密，SRTP加密后的数据，消息格式是一样的

## RTCP/SRTCP
RTP/SRTP和RTCP/SRTCP是一一对应的

发送RTP数据报的时候如果出现了丢包或者抖动，延迟，整个data延迟时间是多少，这些都是通过RTCP数据上报出来的

## DTLS
在对RTP数据进行加密前，需要先进行证书的检验以及加密算法的协商，这些是通过DTLS来进行的

## RTP协议
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728075237164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- 包括RTP内容头，RTP内容体
- 头国定长度为12个字节，非国定长度就是CSRC根据CC来控制的，第一个C代表contributing source 第二个C代表count，contributing source的个数，如果是一个就是16个字节的长度，如果是多个就根据cc的数量来控制

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728080233847.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

## RTP头的含义

- V 代表RTP版本号

- P 当整个RTP包都填充好后，要让他填充一些字节，使他保持32对齐的数值，如果置1代表后面有填充位，如果未置1代表后面没有填充位

- X RTP还可以增加一些扩展头，扩展头就是用户自定义的含义，用户拿到这些扩展头可以自己识别出来，X代表是否有扩展

- CC 场照上面，代表共享者一共有多少个，占4位，最多代表16 2的四次方，16个共享者

- M 标示帧边界，比如一个压缩后的H264帧也是非常大的，我们最多的一个RTP包，包含的字节1500字节还要去掉一些IP头，RTP头，等等，剩下1400字节，所以我们一个包就存1400个字节，压缩后的H264帧可能也达到1M多，所以就要进行分包，分包后的最后一个包，一般M位标示为1，M位还可以根据不同的编解码器的profile的配置表的含义来设定，所以可能包含不同的含义

- PT（重要） playload type，用于区分不同编解码器的数据，比如H264是一个playload type，vp8 vp9分别有自己的playload type，还有一些动态的playload type是可以协商的，每一个playload type使用不同的编解码去对他进行编码解码

- seq number （重要）在传输数据的时候，是一直向上增长的，是连续的，如果传来的2个seq number是不连续的，那说明这个包有可能丢了，也有可能是乱序了。所以通过这个seq number我们就可以进行判断

- timestamp （重要） 时间戳，有时候我们在判断一个帧的时候，因为他分包了，将多个包组成一个帧，那怎么去组，一般的情况下，我们首先判断timestamp，同一个帧的所有的分包的timesatmp是相同的，并且seq number是连续的，找到这2个后，基本上就可以判定这几个包是同一个帧的。在根据H264，在H264内部，又有分包的起始位和结束位，根据这些标示，我们就能将多个分包组成一个完整的帧了，这就是seq number和timestamp的作用

- SSRC （重要）共享者，我共享了一个视频，这个视频就有一个自己的SSRC，如果有一个音频，那视频和音频之间2个SSRC是完全不同的必须是唯一的，同一个视频SSRC发生变化，比如说产生冲突了，因为SSRC是一个随机数，我在共享视频的时候，我首先说我有这样的SSRC，代表是我的这个视频，但是这个时候已经有另外的一个人已经共享了一个音频，或者是其他的媒体，他的SSRC已经跟你是一样的，这个时候就代表冲突了，冲突的时候为了避免冲突，就需要改变SSRC，重新产生一个SSRC,代表还是你的这个源，SSRC是会产生变化的

- CSRC 贡献者，比如我们有三个人同时上麦的时候要进行混音，那混出的这路音他的共享者就有三个人，那每个人的SSRC就都放在这个混音后的CSRC中

