#开启nginx的https服务

首先是协议：
1. SSL：（Secure Socket Layer，安全套接字层），位于可靠的面向连接的网络层协议和应用层协议之间的一种协议层。SSL通过互相认证、使用数字签名确保完整性、使用加密确保私密性，以实现客户端和服务器之间的安全通讯。该协议由两层组成：SSL记录协议和SSL握手协议。
2. TLS：(Transport Layer Security，传输层安全协议)，用于两个应用程序之间提供保密性和数据完整性。该协议由两层组成：TLS记录协议和TLS握手协议。
3. SSL是Netscape开发的专门用户保护Web通讯的，目前版本为3.0。最新版本的TLS 1.0是IETF(工程任务组)制定的一种新的协议，它建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本。两者差别极小，可以理解为SSL 3.1，它是写入了RFC的。

//开启ssl，下面是百度出来的东西。

ssl                        on;

//指定证书

ssl_certificate            \*\*/\*\*/wb.crt;

//写上这个重启nginx时，不需要输入密码

ssl_certificate_key        \*\*/\*\*/wb_nopass.key;

//开启缓存，缓存大小为10m，1m可以保存4000个会话，这里可以保存40000个~

ssl_session_cache          shared:SSL:10m;

//超时时间10分钟

ssl_session_timeout        10m;

//指定适用的协议，去年爆出了SSL3的大漏洞，所以这里不支持这个。

ssl_protocols              TLSv1 TLSv1.1 TLSv1.2;

//其他可以参考http://nginx.org/en/docs/http/ngx_http_ssl_module.html

#另外linux下自己生成证书的方法，window没研究过，自行baidu/google下：

//生成一个RSA的密钥，输入两次密码

openssl genrsa -des3 -out wb.key 1024

//生成一个证书请求，输入密码、国家/省/市等地址、公司名、名字、域名、email等信息，最后密码跟可选公司名直接回车

openssl req -new -key wb.key -out wb.csr

//生成一个不需要密码的key，输入密码

openssl rsa -in wb.key -out wb_nopass.key

//因为没有申请正式的（要花钱的！！！），所以自己给自己签发，输入密码，over！

openssl x509 -req -days 365 -in wb.csr -signkey wb.key -out wb.crt
