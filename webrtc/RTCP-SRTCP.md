## RTCP包
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728083118373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- 首先是在UDP之上，在UDP有个UDP HEADER，后面整个都是RTCP包在RTCP包当中又包括RTCP header RTC DATA两部分

- 如果我们要解析RTCP数据包的时候，首先解掉 UDP header 然后在解掉RTCP header，最后剩下的就是RTCP的DATA数据，这个和RTP是一致的，RTP也是这样的UDP HEADER->RTP HEADER->RTP DATA

## 注意
- 一般的情况下的RTCP的端口为RTP端口+1，RTP端口是偶数，RTCP的是奇数，WebRTC中一个区别，WebRTC中需要进行NAT穿越，如果你是多个端口的话就比较麻烦，所以为了简便，所以RTP和RTCP一般情况下会复用端口，所以RTP和RTCP可能使用的是同一个端口

- 一个RTCP包中一般包括多个报告，RTCP的数据一般比较小，不像RTP，有可能一个H264帧很大，会切成很多RTP包，每一个RTP包只是其中一部分数据，但是对于RTCP来说就不存在这样的问题，因为每个报告比较小， 可能就十几个字节，二十几个字节，所以他一次性可以将多个报告发出去，如果在一个回话中有10个在发送，有100个人在听，在回报告的时候实际就是可以将10源的报告同时发出去，通过一个包，减少了包的发送个数

## RTCP Playload Type
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728084401632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- 200 SR 发送者报告
- 201 RR 接受者报告，发送报告和接受报告基本一样，区别在于发送者增加了发送者描述的信息
-202 SEDS 对于源的描述，在SEDS中重要的内容就是sename。sename就是人类可以识别的一个源的名字，这个源可以对应多个SSRC，某一个时刻只有一个SSRC，但SSRC可以发生变化，比如说我一开始的时候定义了一个SSRC和sename的对应关系，报上去之后有另外一个人也用了这个SSRC，这个时候就产生了冲突了，sename是不变的，ssrc需要换，换完了之后在重新上传，在整个会话中sename是唯一的，ssrc也是唯一的，但是同一个sename可以换多个ssrc

- 203 BYE 结束

- 204 APP 应用自己定义的，他里面有一个子类型，可以定义自己的子类型，主要是根据自己的业务去定义

- 还有一些WebRTC经常常见的，比如ifir，请求一个关键帧，当我进行解码的时候，解码器出错了，由于丢包或者其他原因也好，我现在解不了关键帧了，赶紧发送一个ifir向源请求一个关键帧过来，这个时候源就会发送一个关键帧过来，再从关键帧之后在进行解码，这是防止出错的一个应急方案

- NACK 在发送包的时候发现有丢包了，这个时候发送一个请求说现在有丢包，要给我重新传一个包，我们会发送一个NACK，这也是RTCP中，主要的一个应用场景，用在WebRTC中

## RTCP Header
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728085636500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072809041652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

 - P 填充标识，最后一个填充字节是填充字节(含自己)个数，如果我们这里设置了填充位，那么在我们数据后面的字节是填充，到底填充多少个字节，后面在是一个个填充位，这个字节是包含自己本生的字节

 - RC 接受报告块的个数，可以是0，比如说我是一个SDES,他就没有接受块个数，所以就是0

 - PT PlayType 数据包类型，参照上面介绍

 - length 包长度(包长度和填充)(N-1)4个字节，如果我们计算真是长度，(N+1)*4,这是真实长度，其中包括了头部，填充位，数据本身，N代表这个length的长度

 - SSRC 发送者的SSRC，这个包是谁发送，就代表他的SSRC

 ## RTCP Sender Report

- Sender Information block 发送者自己的信息块
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728090629971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

## 前64位 
- 第一个32位是高字节，后面的32位是低字节，它整个代表是64位的timestamp，这是NTP的timestamp，也就是说绝对时间，到底几分几秒几毫秒

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728090740374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)


- RTP时间戳 RTP数据包在发送过程中的一个相对时间 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728091038257.png)

- 发送者发送的包数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728091147273.png)

- 发送整个的字节数
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072809121260.png)

## Send Info 说明

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728091257689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- NTP 网络时间戳，用于不同源之间的同步，现在的计算机的时间，几分几秒，都是通过NTP获取到的，用于不同源之间的同步，比如音频和视频之间的同步，就的通过NTP，我到底是几分几秒发的数据，我的口型对应的声音要能对的上

- RTP timestamp 相对时间戳，与RTP包时间戳一致，他从起始位置可能是随机的值，每发一个就增加一个date的时间，从那个值加上date，就是他的时间戳

- packet count 总发包数，SSRC变化时被重置，当我这个共享源没有发生变化时，我一共发送多少个包，从我一开始发送到我现在一共发送多少包，怎么计算呢，RTP中有一个seq number，第一个包的seq number，可能是一个随机数，到我最近这次发送的RTP的seq number，进行相减，就找到了所有的包的发送数量

- octet count 总共发送字节数，整个在发送的过程中，发的每个包有多少个字节，累加出来的


## Receiver Report block 
发送者也是接受者的话，那我接受的数据报告也要发送出去

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803074512579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)


- SSRC_1 SSRC_2有其他的源发送的数据，做不同的统计
- fraction lost 丢包率
- cumulative number of packets lost C 丢包的数量
- extended highest sequence number received EHSN 这个是sequence number 的高字节位
- inter-arrival jitter J 延迟大小
- last SR LSR 最后一个SR，上一次的SR
- delay slnce last SR DLSR 自从上一次的SR的delay时间

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803075049384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- SSRC_n 如果有多个1234567，不是_1234567，每一个SSRC都是不同的
- 从上一次的报告到现在一共发送100个包丢了5个包，丢包率为百分之五
- 现在的seq num是1000，起始是100，实际总的包数是900个包，在900个包里一共丢了多少个包，这是一个累积的值

webRTC会通过这些参数去检验网络质量，如果网络质量有问题，他就会减少发包量，做流控，防止发包过大，将整个网络阻塞

## RTCP Receiver Report
- 只包括Receiver Report block

RTCP SR/RR 发送时机
- 接收端只发送RR，如果我不发任何数据的时候，我收到多少数据，定时发RR的报文，整个网络的质量是什么样子的，进行上报

- 既是发送端又是接收端时，在上一次报告之后有发送过数据时，则发SR

- SR和RR都是为了上报数据，一个是说如果我已经发送过数据，这时候我连发送数据在由我接受数据的报告，我都发送出去。但是我只是接受了，但是没有发送任何数据的时候我只发送RR