介绍：

v2ray（Xray） 前置（监听443端口），trojan+tcp 以 h2 或 http/1.1 自适应协商连接，非 v2ray（Xray） 的 web 连接回落给 caddy2，实现了兼容 trojan，即可直接使用 trojan 客户端连接。

原理图：v2ray\trojan client <------ tcp+tls ------> v2ray server <- web回落 -> caddy2

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。 

2、caddy2 等于或大于 v2.3.0 版才支持 Caddyfile 配置开启 h2c server。

3、caddy2 支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

4、caddy2 发行版不支持 PROXY protocol（接收）。如要支持 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译，或下载本人 github 中编译好的 caddy2 来使用即可。

5、本示例中 caddy2 的 Caddyfile 格式配置与 json 格式配置二选一即可（效果一样）。

6、配置1：端口回落，没有启用 PROXY protocol。配置2：进程回落，没有启用 PROXY protocol。配置3：进程回落，启用了 PROXY protocol。
