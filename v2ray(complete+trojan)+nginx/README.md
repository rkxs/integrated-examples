介绍：

利用 nginx 支持 SNI 分流特性，对 vless+tcp 与 trojan+tcp 进行 SNI 分流（四层转发），实现除 v2ray kcp 外共用443端口。vless+tcp 以 h2 或 http/1.1 自适应协商连接，分流 ws（WebSocket）连接，非 v2ray 的 web 连接回落给 nginx。trojan+tcp 以 h2 协商连接，非 v2ray 的 web 连接也回落给 nginx。其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它ws类应用，参考反向代理ws类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、trojan+tcp+tls（回落配置。）

5、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

v2ray tcp 类应用直连，且以 http/2 或 http/1.1 自适应代理科学上网；v2ray ws（WebSocket）类应用分流一次。

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。

2、nginx 预编译程序包一般不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

3、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用），故 vless+tcp 或 trojan+tcp 的端口回落或进程回落必须分开，分别对应 http/1.1 与 h2 回落。而 trojan+tcp 目前不支持 http/1.1 与 h2 的端口回落或进程回落分离 ，故 trojan+tcp 连接只能二选一。本次示例 trojan+tcp 采用 h2 连接及回落，毕竟 h2 连接自带链路复用，且延迟小一点。

4、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module 与 stream_realip_module 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

5、此方法采用的是 SNI 方式实现共用 443 端口，支持 vless+tcp 与 trojan+tcp 完美共存，支持各自 xtls 应用，但需多个域名（多个证书或通配符证书）来标记分流。

6、配置4：端口转发、端口回落\分流及 nginx SNI 的端口分流，没有启用 PROXY protocol。配置5：进程转发、进程回落\分流及 nginx SNI 的进程分流，没有启用 PROXY protocol。配置6：进程转发、进程回落\分流及 nginx SNI 的进程分流，启用了 PROXY protocol。
