如果eclipse是用的server的方式，设置步骤如下：
1. 点击eclipse上的run图标旁边的下拉箭头
2. 然后选择Run Configurations,
3. 系统弹出设置tomcat配置页面，在Argument中末尾添加参数中的VM arguments中追加：
```
-Xms1024M -Xmx2048M -XX:PermSize=1024m -XX:MaxPermSize=2048m
```
注意前面有一个空格。
如果是tomcat单独配置，设置步骤如下：
* Linux：
在/usr/local/apache-tomcat/bin目录下的catalina.sh
添加：JAVA_OPTS=''-Xms1024m -Xmx2048m''
* Windows：
在catalina.bat最前面加入
set JAVA_OPTS=-Xms1024m -Xmx2048m
