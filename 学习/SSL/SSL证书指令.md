openssl genrsa -aes256 -out rootkey.pem 2048
生成根证书的密匙。
openssl req -sha256 -x509 -new -key rootkey.pem -out root.crt -subj "/C=CN/ST=HN/L=ZZ/O=CMCC/OU=CMCC/CN=CMCC"
生成根证书。注意-x509，与步骤4和7不同。需要输入机构相关信息。
openssl genrsa -aes256 -out clientkey.pem 2048
生成客户端的密匙。
openssl req -sha256 -new -key clientkey.pem -out client.csr -subj "/C=CN/ST=HN/L=ZZ/O=CMCC/OU=CMCC/CN=localhost"
生成客户端证书的请求文件。请求根证书来签发。
openssl x509 -req -sha256 -in client.csr -CA root.crt -CAkey rootkey.pem -CAcreateserial -days 3650 -out client.crt
用根证书来签发客户端请求文件，生成客户端证书client.crt。
openssl genrsa -aes256 -out serverkey.pem 2048
生成服务器端的密匙。
openssl req -sha256 -new -key serverkey.pem -out server.csr -subj "/C=CN/ST=HN/L=ZZ/O=CMCC/OU=CMCC/CN=localhost"
生成服务器端证书的请求文件。请求根证书来签发。
openssl x509 -sha256 -req -in server.csr -CA root.crt -CAkey rootkey.pem -CAcreateserial -days 3650 -out server.crt
用根证书来签发服务器端请求文件，生成服务器端证书server.crt。
openssl pkcs12 -export -in client.crt -inkey clientkey.pem -out client.pkcs12
打包客户端资料为pkcs12格式(client.pkcs12)。需要输入密码，请记住。
openssl pkcs12 -export -in server.crt -inkey serverkey.pem -out server.pkcs12
打包服务器端资料为pkcs12格式(server.pkcs12 )。需要输入密码，请记住。
keytool -importkeystore -srckeystore client.pkcs12 -destkeystore client.jks -srcstoretype pkcs12
生成客户端keystore(client.jks)。使用keytool的importkeystore指令。pkcs12转jks。需要pkcs12密码和jks密码。
keytool -importkeystore -srckeystore server.pkcs12 -destkeystore server.jks -srcstoretype pkcs12
生成服务器端keystore(server.jks)。使用keytool的importkeystore指令。pkcs12转jks。需要pkcs12密码和jks密码。
keytool -importcert -keystore server.jks -file root.crt
这一步不一定需要的。我发现服务器使用JDK6和JDK7的时候，必须要把根证书加到服务器证书里面，否则交谈失败。nul certification。Google里面很多结果，但是都不灵光。
后记
理解概念是最重要的，路有很多条。以上指令还缺少几条，用来生成服务器端的trustkeystore，客户端的trustkeystore。trustkeystore里面不包含私匙。
参考链接
OpenSSL官方文档
keytool说明书
补上TrustKeyStore指令
keytool -importcert -alias ca -file root.crt -keystore clienttrust.jks
生成Client端的对外KeyStore。先把根证书放到里面。
keytool -importcert -alias clientcert -file client.crt -keystore clienttrust.jks
把Client证书加到对外KeyStore里面。
keytool -importcert -alias ca -file root.crt -keystore servertrust.jks
生成Server端的对外KeyStore。先把根证书放到里面。
keytool -importcert -alias servercert -file server.crt -keystore servertrust.jks
把Server证书加到对外KeyStore里面。
