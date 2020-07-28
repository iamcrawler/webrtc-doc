在集群的模式下会随即选择一座桥 负载均衡算法 随机
jitsi/jicofo/bridge/SplitBridgeSelectionStrategy.java

[考虑视频桥数量的pr](https://github.com/jitsi/jicofo/pull/418)

Perhaps increasing the bridges' reporting rate and adding a small interval between clients joining will be a better solution for performance testing.
（也许提高网桥的报告率并在客户端加入之间增加一个较小的间隔将是性能测试的更好解决方案。）

apt-get install --no-install-recommends jitsi-meet，这将跳过turnserver的安装。

我发现我的listen 443
Nginx 主机conf具有But /etc/nginx/modules-enabled/60-jitsi-meet.conf也具有一个listen 443似乎是用于新coturn安装的conf文件。
因此似乎是有冲突的设置

service jitsi-videobridge2 restart
service jitsi-videobridge2 status
 cat /var/run/jitsi-videobridge/jitsi-videobridge.pid is still ok no “2” needed here


[老外写的安装jitsi2的文档](https://lists.tetaneutral.net/pipermail/technique/2020-March/003823.html)

```
jvb.log
2020-04-01 23:08:11.815 INFO: [27] [hostname=localhost id=shard] MucClient.lambda$getConnectAndLoginCallable$7#648: Logging in. 2020-04-01 23:08:11.815 SEVERE: [27] RetryStrategy$TaskRunner.run#198: org.jivesoftware.smack.sasl.SASLErrorException: SASLError using SCRAM-SHA-1: not-authorized org.jivesoftware.smack.sasl.SASLErrorException: SASLError using SCRAM-SHA-1: not-authorized at org.jivesoftware.smack.SASLAuthentication.authenticationFailed(SASLAuthentication.java:292) at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1100) at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$300(XMPPTCPConnection.java:1000) at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:1016) at java.lang.Thread.run(Thread.java:748)
jicofo.log
Jicofo 2020-04-01 23:18:35.321 INFO: [207] org.jitsi.jicofo.xmpp.FocusComponent.handleConferenceIq().401 Focus
````
解决办法:
jvb.dat文件与focus.dat位于同一目录中。
在我看到的dat文件中
accounts/jvb.dat return { ["password"] = "ok"; };
，在jvb config中，/ etc / jitsi / videobridge / config
JVB_SECRET=0ya1rJN2

org.jitsi.videobridge.NAT_HARVESTER_PUBLIC_ADDRESS在JVB2上已经过时


先关闭jitsi-videobridge模块，启动nginx，然后再次启动jitsi-videobridge模块，这时jitsi-videobridge模块就会改用4443端口

修改jitsi的徽章log替换
```
/usr/share/jitsi-meet/images/watermark.png
/usr/share/jitsi-meet/images/favicon.ico
/usr/share/jitsi-meet/favicon.ico
```

修改jitsi的网站的标题
```
/usr/share/jitsi-meet/title.html
```

修改APP的名字，进入房间的欢迎语以及log徽章跳转的页面
```
/root/.jitsi-meet-cfg/web/interface_config.js
```
jitsi-meet-config配置
参考连接：https://gist.github.com/maps90/e06043734cda7bb67392ce2599ae25d4