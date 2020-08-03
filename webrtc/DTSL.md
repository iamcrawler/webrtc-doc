## RTCP SDES的消息结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803080712485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)


## SDES Item 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803081130578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- CNAME 如果等于1.他有一个长度。他是t=lv的格式，t=cname，

## SDES 说明
- SR SSRC/CSRC 数量
- Item采用TLV存放数据 T=type类型 L=length长度 V=value
- CNAME SSRC的规范名，在RTP会话中唯一

对于Webrtc 基本上不用sdes消息，因为我们的cnmae，在sdp中已经有came的描述，而且每个的cname所对应的SSRC也是有值的，除非在我们通话中的SSRC发生变化，他才需要修改，否则在我们交换SDP的时候，就已经知道cnmae对应的SSRC了。一开始交换媒体信息的时候就能通讯，除非我们的音视频源发生了中断，有可能在发送SDES然后在去重新进行绑定。

## RTCP BYE
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803081911215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- SC SSRC的count
- ... 代表了一系列的我要关闭的流
- reason for leaving(opt) 离开的原因，类型是字符串

## RTCP APP
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803082244140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

- 在这里把SC变成了ST，也就是sub type 
- application-dependent data 自定义的数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803082440134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

## DTLS
在WebRTC要使用SRTP的时候，实际要进行证书的交换，证书交换完了后有秘钥的交换，交换完了之后才能进行加密数据解密

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803082822897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)

首先一段发送ClientHello,另一端发送ServerHello，之后开始交换证书，B把证书交换给A，A也要把证书交换给B，在双方拿到证书之后，要怎么证明证书是ok的，是代表这个人的，有没有可能第三个人插进来解析了证书之后给你换了呢，在DTLS之前，实际我们进行过媒体协商SDP，在媒体协商里面有一个 fingerprint(指纹)，指纹实际就是对证书的唯一标示，首先双方都把对方的证书指纹先发给对方，这时候交换证书之后，通过指纹与这个证书重新进行验证，经过验证之后，指纹和证书能保持一致，就能证明这个人是ok的

## SRTP
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200803083525473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODgwMDg3,size_16,color_FFFFFF,t_70)