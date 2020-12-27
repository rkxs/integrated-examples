介绍：

除 v2ray kcp 外，所用应用共用443端口。此端口由 caddy2 监听（即 caddy2 前置），反向代理 v2ray 或 Xray 的 ws（WebSocket） 与 h2（http/2），若有 naiveproxy 就进行正向代理。无 vless+tcp 应用。包括应用如下：
  
1、vless+ws+tls（tls由caddy2提供及处理，不需配置；另可改成或添加vmess+ws+tls、SS+v2ray-plugin+tls、trojan+ws+tls应用。）

2、SS+v2ray-plugin+tls（tls由caddy2提供及处理，不需配置；另可改成或添加vless+ws+tls、vmess+ws+tls、trojan+ws+tls应用。）

3、vless+h2c+tls（tls由caddy2提供及处理，不需配置；另可改成或添加vmess+h2c+tls应用。）

4、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

5、naiveproxy （带有forwardproxy插件的caddy2才有naiveproxy应用，否则仅上边应用。tls由caddy2提供及处理。）

注意：

1、caddy2 等于或大于 v2.2.0-rc.1 版才支持 h2c proxy，即支持 v2ray 的 h2（http/2）反向代理。

2、使用本人 github 中编译好的 caddy2 文件，才可同时支持 naiveproxy、ws（WebSocket）与 h2（http/2）反向代理。

3、2、caddy2 的 Caddyfile 配置与 caddy.json 配置二选一（效果一样）。支持自动 https，即自动申请证书与私钥，且自动更新，自动 http 重定向到 https。

4、协议 vless 或 vmess 采用 WebSocket 传输方式支持 Unix domain socket 转发，即进程转发。

5、配置1：全部端口转发。配置2：vless+ws进程转发，其它端口转发。
