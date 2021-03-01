一、分别回落 caddy2 不同网站的配置方法

此方法解决 v2ray 或 Xray 前置监听443后，不影响原来 caddy2 前置时，不同域名访问不同网站问题。

注意：

1、若不同域名没有使用通配符证书，那么还需要在 v2ray 或 Xray 中并列配置多个域名对应的证书及私钥。

2、此回落到不同网站是 v2ray 或 Xray 解除 tls 后 caddy2 进行的 host（域名）分流。

3、caddy2 json 配置才支持此应用， Caddyfile 配置不支持。因 Caddyfile 配置参数是简化的，非完整的。

4、也可以用 caddy2 SNI 或 haproxy SNI 等分流来解决问题（不同方法，达到相同效果。）。caddy2 SNI 配置示例见如下介绍。haproxy SNI 配置示例参考示例中用 haproxy SNI 分流的 haproxy 配置。

二、caddy2 SNI 分流的配置方法

此方法也可以解决 v2ray（Xray）应用与网站应用（原网站不想做回落网站，或 nginx/caddy2 等有多个网站应用。）共用443端口问题。

注意：

1、caddy2 加 caddy-l4 插件定制编译的才可以实现 SNI 分流，目前仅支持使用 json 配置。

2、采用改进的 caddy-l4 插件定制编译的才同时支持 PROXY protocol（发送），且可以对分流的进程或端口分别开启 PROXY protocol（发送）。

3、1_SNI_caddy.json 分流采用 local loopback 应用（redirect），实现转发端口（域名）的分流，简称 caddy2 SNI 的端口分流。此端口分流效率稍低，可适用全部服务器。

4、2_SNI_caddy.json 分流采用 Unix Domain Socket 应用，实现转发进程（域名）的分流，简称 caddy2 SNI 的进程分流。此进程分流效率高，但在 Windows 10 Build 17036 前不可用。

5、本人 github 中的相关配置示例已配置 caddy2 SNI 分流共用端口 ，此配置方法仅备份及参考等。

三、caddy2 以 DNS API 方式申请证书与私钥

1、dnspod、cloudflare、dnspodcn 以 DNS API 方式申请证书与私钥，可以申请普通证书，也可以申请通配符证书。

2、v2ray（Xray）可以直接使用 /home/tls/certificates/acme-v02.api.letsencrypt.org-directory/wildcard_.xx.yy 路径及目录中通配符证书及私钥。

注意：

1、dnspod 分 dnspod.com（国际版）与 dnspod.cn（中国版），故两者插件不通用，必须对应各自 dnspod 域名解析使用。

2、cloudflare 插件目前已不支持 freenom 免费后缀的域名了。

3、v2ray（Xray）可以直接使用 caddy2 以 DNS API 方式申请的证书与私钥，配合 Xray 服务端（版本必须不低于 v1.3.0）更新 OCSP 数据前自动检查并重载证书与私钥，可实现 Xray 服务端证书与私钥的申请及更新自动化；否则 v2ray（Xray）服务端（Xray 版本低于 v1.3.0）不支持自动热重载证书，caddy2 证书到期更新后需手动重启 v2ray（Xray）来重新加载更新的证书。

4、推荐采用 json 配置，否则采用 Caddyfile 配置必须启用无用端口来自动申请及更新证书与私钥。

四、naiveproxy 服务端使用 Caddyfile 配置 PROXY protocol 等方法 

此 naive_Caddyfile 模板实现了 naiveproxy 应用开启 PROXY protocol、http/3及 h2c server 的支持。

注意：

1、caddy2 等于或大于 v2.3.0版才支持 Caddyfile 配置开启 h2c server。

2、使用本人 github 中编译好的 caddy2 文件（采用当前最新proxyprotocol插件编译的），才支持使用 Caddyfile 配置开启 PROXY protocol 支持。
