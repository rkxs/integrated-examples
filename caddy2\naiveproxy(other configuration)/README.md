一、分别回落 caddy2 不同网站的配置方法

此方法解决 v2ray 或 Xray 前置监听443后，不影响原来 caddy2 前置时，不同域名访问不同网站问题。

注意：

1、若不同域名没有使用通配符证书，那么还需要在 v2ray 或 Xray 中并列配置多个域名对应的证书及私钥。

2、caddy2 json 配置才支持此应用， Caddyfile 配置不支持。因 Caddyfile 配置参数是简化的，非完整的。

3、也可以用 haproxy 或 v2ray SNI 等分流来解决问题（不同方法，达到相同效果。）。haproxy SNI 配置示例参考示例中用 haproxy SNI 分流的 haproxy 配置。v2ray SNI 配置示例参考 ‘v2ray(other configuration)’ 中 SNI_redirect_config.json 或 SNI_domainsocket_config.json 配置。

二、naiveproxy 服务端使用 Caddyfile 配置 PROXY protocol 等方法

此 naive_Caddyfile 模板实现了 naiveproxy 应用开启 PROXY protocol、http/3及 h2c server 的支持。

注意：

1、caddy2 等于或大于 v2.3.0版才支持 Caddyfile 配置开启 h2c server。

2、使用本人 github 中编译好的 caddy2 文件（采用改进的proxyprotocol插件编译的），才支持使用 Caddyfile 配置开启 PROXY protocol 支持。
