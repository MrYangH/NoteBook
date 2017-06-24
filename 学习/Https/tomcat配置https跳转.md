在tomcat的server.xml文件中，找到https的配置，去掉注释后修改，参考如下
```xml
<Connector SSLEnabled="true" clientAuth="true" keystoreFile="/Users/huhu/z/server.jks" keystorePass="123456" keystoreType="JKS" maxThreads="150" port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol" scheme="https" secure="true" sslProtocol="TLS" truststoreFile="/Users/huhu/z/servertrust.jks" truststorePass="123456" truststoreType="JKS"/>
```
另外，把配置文件中所有端口号8443都替换为443，这样才能不附带端口号默认访问https，类似8080改为80。
在tomcat的web.xml文件里最后面，后面加上这样一段
```xml
<login-config>  
    <!-- Authorization setting for SSL -->  
    <auth-method>CLIENT-CERT</auth-method>  
    <realm-name>Client Cert Users-only Area</realm-name>  
</login-config>  
<security-constraint>  
    <!-- Authorization setting for SSL -->  
    <web-resource-collection >  
        <web-resource-name >SSL</web-resource-name>  
        <url-pattern>/*</url-pattern>  
    </web-resource-collection>  
    <user-data-constraint>  
        <transport-guarantee>CONFIDENTIAL</transport-guarantee>  
    </user-data-constraint>  
</security-constraint>
```
其中，在在tomcat的server.xml文件的https配置中，clientAuth="true"是双向认证，clientAuth="false"是单向认证。
单向认证，就是浏览器(客户端)认证服务器的证书即可。
双向认证，不仅浏览器(客户端)要认证服务器的证书，服务器也要认证浏览器(客户端)的证书。
