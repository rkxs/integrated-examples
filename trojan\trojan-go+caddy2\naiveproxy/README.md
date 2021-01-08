介绍：

本配置是 trojan 或 trojan-go 应用，以 h2 或 http/1.1 自适应协商连接，非trojan 或 trojan-go 的 https 连接回落给 caddy2；若有 naiveproxy，caddy2 再进行正向代理。其应用如下：

1、trojan(trojan-go)（回落配置。）

2、naiveproxy （带有forwardproxy插件的caddy2才支持naiveproxy应用，否则仅上边应用。tls由trojan(trojan-go)提供及处理，不需配置。）

原理图： trojan\trojan-go client <------ https ------> trojan\trojan-go server <- web回落 -> caddy2\naiveproxy

注意：

1、caddy2 等于或大于 v2.3.0 版才支持 Caddyfile 配置开启 h2c server，但 caddy2 Caddyfile 配置不支持进程（Unix Domain Socket 应用）监听。

2、caddy2 Caddyfile 配置支持 http/1.1 server 与 h2c server 共用一个端口。caddy2 json 配置支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

3、因 trojan(trojan-go) 不支持 Unix Domain Socket，故不能采用进程回落。

4、因 trojan(trojan-go) 不支持 PROXY protocol（发送），故不启用此项应用。

5、使用本人 github 中编译好的 caddy2 文件，才支持 naiveproxy 应用。

6、trojan-go 使用 go 实现了完全兼容 trojan，还有自己的特色：trojan-go 支持使用多路复用提升并发性能，使用路由模块实现国内直连；支持 CDN 流量中转(基于 WebSocket over TLS/SSL )；支持使用 AEAD 对 trojan 流量二次加密(基于 Shadowsocks AEAD )；支持可插拔的传输层插件，允许替换 TLS，使用其他加密隧道传输 trojan 协议流量。
