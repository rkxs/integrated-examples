介绍：

此配置包括 v2ray、naiveproxy(caddy2) 及 trojan(trojan-go) 应用。用 haproxy 或 nginx 为 v2ray、naiveproxy(caddy2)、trojan(trojan-go) 进行 SNI 分流（四层转发），实现共用443端口。另 caddy2 还同时为 vless+tcp 与 trojan(trojan-go) 提供回落服务，为 vless/vmess+h2c 提供反向代理，为 naiveproxy 提供正向代理。v2ray 包括应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它WebSocket类应用，参考反向代理WebSocket类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、vless+h2c+tls（tls由caddy2提供及处理，不需配置；另可改成或添加vmess+h2c+tls应用。）

5、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、caddy2 目前只能 json 配置才能开启 h2c server，故要实现 h2 回落就不能采用 Caddyfile 配置；另外caddy2 版本不能低于 v2.1.0 ，否则不支持 h2c server。

2、caddy2 支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

3、caddy2 等于或大于 v2.2.0-rc.1 版才支持 h2c proxy，即支持 v2ray 的 h2（http/2）反向代理。

4、caddy2 发行版不支持 PROXY protocol。如要支持 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译。

5、使用本人 github 中编译好的 caddy2 文件，才可同时支持 naiveproxy、h2 回落、h2（http/2）反向代理及 PROXY protocol的应用。

6、nginx 预编译程序包一般不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

7、因 trojan(trojan-go) 不支持 PROXY protocol（接收），而 nginx SNI 中的 PROXY protocol 发送是针对共用端口全局模式，故配置3不再使用 nginx。

7、因 trojan(trojan-go) 不支持 PROXY protocol（发送），故 trojan(trojan-go) 回落不开启 PROXY protocol 支持。

8、因 trojan(trojan-go) 不支持 Unix Domain Socket，故只能全部端口回落及针对自己端口分流。

9、配置1：端口转发、端口回落及 haproxy 或 nginx SNI 的端口分流，没有启用 PROXY protocol。配置2：进程转发、端口回落及 haproxy 或 nginx SNI 的进程分流（trojan除外），没有启用 PROXY protocol。配置3：进程转发、端口回落及 haproxy SNI 的进程分流（trojan除外），启用了 PROXY protocol（trojan除外）。
