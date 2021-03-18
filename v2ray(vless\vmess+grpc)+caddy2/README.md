介绍：

通过 caddy2 前置 v2ray server 实现 grpc+tls（http/2）反向代理，tls 由 caddy2 提供及处理；包括 vless+grpc+tls 与 vmess+grpc+tls 两种应用。

原理图： v2ray client <------ grpc+tls ------> caddy2 <- h2c -> v2ray server

注意：
1、Xray 版本不小于1.40，v2ray 版本不小于v4.36.2，才完美支持grpc应用。

2、caddy2 等于或大于 v2.2.0-rc.1 版才支持 h2c proxy，即支持 v2ray 的 h2（http/2）反向代理。

3、caddy2 的 Caddyfile 配置与 caddy.json 配置二选一（效果一样）。支持自动 https，即自动申请与更新证书与私钥，自动 http 重定向到 https。

4、支持grpc的CDN加速。
