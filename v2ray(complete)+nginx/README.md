介绍：

v2ray（Xray） 前置（监听443端口），利用 vless+tcp 强大的回落/分流特性，实现除 v2ray（Xray） kcp 外共用443端口。vless+tcp 以 h2 或 http/1.1 自适应协商连接，分流 ws（WebSocket）连接，非 v2ray（Xray） 的 web 连接回落给 nginx。其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它ws类应用，参考反向代理ws类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故此 vless+tcp 应用中的回落端口或进程必须分开，分别对应。

2、nginx 不支持 h2c proxy，故 nginx 不能实现 v2ray 的 h2（http/2）反向代理。

3、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module（必须加） 及 stream_realip_module（可选加） 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

4、配置1：端口转发、端口回落\分流，没有启用 PROXY protocol。配置2：进程转发、进程回落\分流，没有启用 PROXY protocol。配置3：进程转发、进程回落\分流，启用了 PROXY protocol。
