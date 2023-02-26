# openssl生成证书server.key server.crt 
Key是私用秘钥，通常是RSA算法

Csr是证书请求文件，用于申请证书。在制作csr文件时，必须使用自己的私钥来签署申，还可以设定一个密钥。

crt是CA认证后的证书文，签署人用自己的key给你签署凭证。

## key的生成
openssl genrsa -out server.key 2048

这样是生成RSA密钥，openssl格式，2048位强度。server.key是密钥文件名。

## csr的生成
openssl req -new -key server.key -out server.csr，需要依次输入国家，地区，组织，email。最重要的是有一个common name，可以写你的名字或者域名。如果为了https申请，这个必须和域名吻合，否则会引发浏览器警报。生成的csr文件讲给CA签名后形成服务端自己的证书。

## crt的生成
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

生成的server.crt文件


## SSL Error: Self signed certificate
解决方案： http://gagravarr.org/writing/openssl-certs/others.shtml#ca-openssl