介绍：

除 v2ray kcp 外，所用应用共用443端口。此端口由 nginx 监听（即 nginx 前置），反向代理 ws（WebSocket）。v2ray 包括应用如下：

1、vless+ws+tls（tls由nginx提供及处理，不需配置；另可改成或添加vmess+ws+tls、SS+v2ray-plugin+tls、trojan+ws+tls应用。）

2、SS+v2ray-plugin+tls（tls由nginx提供及处理，不需配置；另可改成或添加vless+ws+tls、vmess+ws+tls、trojan+ws+tls应用。）

3、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、nginx 不支持 h2c proxy，故 nginx 不能实现 v2ray 的 h2（http/2）反向代理。

2、若系统版本过低，其对应发行版仓库自带 nginx 预编译程序包可能不支持 tls1.3；如需要支持 tls1.3，必须先升级 OpenSSl 版本大于 1.1.1，再进行 nginx 源代码编译和安装。

3、协议 vless 或 vmess 采用 WebSocket 传输方式支持 Unix domain socket 转发，即进程转发。

4、配置1：全部端口转发。配置2：vless+ws进程转发，其它端口转发。
