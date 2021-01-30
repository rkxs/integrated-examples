介绍：

此示例包括 v2ray、trojan（trojan-go）应用。利用 nginx 支持 SNI 分流特性，对 v2ray（vless+tcp）、trojan（trojan-go）进行 SNI 分流（四层转发），实现除 v2ray kcp 外共用443端口；同时为 v2ray（vless+tcp）与 trojan（trojan-go）提供 web 回落服务。v2ray 包括如下应用：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它ws类应用，参考反向代理ws类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、vmess+kcp+seed（可改成vless+kcp+seed，或添加它。）


注意：

1、nginx 预编译程序包一般不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

2、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故此 vless+tcp 应用中的回落端口或进程必须分开，分别对应。

3、nginx 不支持 h2c proxy，故 nginx 不能实现 v2ray 的 h2（http/2）反向代理。

4、因 trojan(trojan-go) 不支持 Unix Domain Socket，故 trojan(trojan-go) 不启用此项应用，从而回落部分仅端口回落及端口监听。

5、因 nginx SNI 中的 PROXY protocol（发送）是针对共用端口全部开启（全局模式），而 trojan(trojan-go) 不支持 PROXY protocol（接收与发送），故所有配置不启用此项应用。

6、此方法采用的是 SNI 方式实现共用 443 端口，支持 v2ray（vless+tcp）、trojan（trojan-go）完美共存，支持各自特色应用，但需多域名（多证书或通配符证书）来标记分流。

7、配置1：端口转发、端口回落\分流及 nginx SNI 的端口分流，没有启用 PROXY protocol。配置2：进程转发、端口回落\分流及 nginx SNI 的进程分流（trojan除外），没有启用 PROXY protocol。
