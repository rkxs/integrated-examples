介绍：

此示例包括 v2ray、naiveproxy（caddy2）、trojan（trojan-go）应用。利用 caddy2 支持 SNI 分流特性，对 v2ray（vless+tcp）、naiveproxy（caddy2）、trojan（trojan-go）进行 SNI 分流（四层转发），除 v2ray kcp 外实现共用443端口。caddy2 同时为 v2ray（vless+tcp）与 trojan（trojan-go）提供 web 回落服务，为 v2ray（vless/vmess+h2c）提供反向代理，为 naiveproxy 提供正向代理。v2ray 包括应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它ws类应用，参考反向代理ws类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、vless+h2c+tls（tls由caddy2提供及处理，不需配置；另可改成或添加vmess+h2c+tls应用，参考反向代理h2类的单一示例。）

5、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、caddy2 加 caddy-l4 插件定制编译的才可以实现 SNI 分流，目前仅支持使用 json 配置。特别提醒：采用改进的 caddy-l4 插件定制编译的才同时支持 PROXY protocol（发送），且可以对进程或端口分别开启 PROXY protocol（发送）。

2、caddy2 等于或大于 v2.2.0-rc.1 版才支持 h2c proxy，即支持 v2ray 的 h2（http/2）反向代理。

3、caddy2 支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

4、caddy2 发行版不支持 PROXY protocol（接收）。如要支持 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译，或下载本人 github 中编译好的 caddy2 来使用即可。

5、使用本人 github 中编译好的 caddy2 文件，才可同时支持 SNI 分流、naiveproxy、h2c server、h2c proxy及 PROXY protocol 等应用。

6、因 trojan(trojan-go) 不支持 Unix Domain Socket，故 trojan(trojan-go) 不启用此项应用，从而回落部分仅端口回落及端口监听。

7、因 trojan(trojan-go) 不支持 PROXY protocol（接收与发送），故 trojan(trojan-go) 不启用此项应用，从而回落部分不启用 PROXY protocol（接收与发送）。

8、配置4：端口转发、端口回落\分流及 caddy2 SNI 的端口分流，没有启用 PROXY protocol。配置5：进程转发、端口回落\分流及 caddy2 SNI 的进程分流（对trojan除外），没有启用 PROXY protocol。配置6：进程转发、端口回落\分流及 caddy2 SNI 的进程分流（对trojan除外），启用了 PROXY protocol（回落部分除外）。

9、本示例不推荐同时提供网站服务，否则 caddy2 压力过大。
