介绍：

此配置包括 v2ray、naiveproxy（caddy2）应用。利用 haproxy 或 nginx 支持 SNI 分流特性，对 v2ray（vless+tcp）、v2ray（trojan+tcp）、naiveproxy（caddy2）进行 SNI 分流（四层转发），实现除 v2ray kcp 外共用443端口。另 caddy2 为 v2ray（vless+tcp）与 trojan+tcp 提供回落服务，为 v2ray（vless/vmess+h2c）提供反向代理，为 naiveproxy 提供正向代理。v2ray 包括应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它ws类应用，参考反向代理ws类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、vless+h2c+tls（tls由caddy2提供及处理，，不需配置；另可改成或添加vmess+h2c+tls应用，参考反向代理h2类的单一示例。）

5、trojan+tcp+tls（回落配置。）

6、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

v2ray vless+tcp 类应用直连，v2ray ws（WebSocket）类应用分流一次；v2ray trojan+tcp 直连；naiveproxy 直连，v2ray h2（http/2）类应用分流（反代）一次。

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。 

2、caddy2 等于或大于 v2.2.0-rc.1 版才支持 h2c proxy，即支持 v2ray 的 h2（http/2）反向代理。

3、caddy2 等于或大于 v2.3.0 版才支持 Caddyfile 配置开启 h2c server。

4、caddy2 支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

5、caddy2 发行版不支持 PROXY protocol（接收）。如要支持 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译，或下载本人 github 中编译好的 caddy2 来使用即可。特别提醒：采用改进的 proxyprotocol 插件定制编译，才支持使用 Caddyfile 配置，否则只能使用 json 配置。

6、本示例中 naiveproxy(caddy2) 的 naive_Caddyfile 配置虽然可用，但会产生很多报错日志（暂不能解决）。

7、使用本人 github 中编译好的 caddy2 文件，才可同时支持 naiveproxy、h2c server、h2c proxy 及 PROXY protocol 等应用。

8、nginx 预编译程序包一般不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

9、配置1：端口转发、端口回落\分流及 haproxy 或 nginx SNI 的端口分流，没有启用 PROXY protocol。配置2：进程转发、进程回落\分流及 haproxy 或 nginx SNI 的进程分流，没有启用 PROXY protocol。配置3：进程转发、进程回落\分流及 haproxy 或 nginx SNI 的进程分流，启用了 PROXY protocol。

10、若采用配置3、且使用 nginx SNI 来分流的，又想 naiveproxy 开启 http/3 代理支持，可参考配置1对应 naiveproxy 部分配置：把进程转发改成端口转发，且 naiveproxy http/3 开启即可。

11、若除了实现综合的科学上网，还需提供实际网站服务，推荐本示例。网站服务可由nginx或caddy2提供服务。
