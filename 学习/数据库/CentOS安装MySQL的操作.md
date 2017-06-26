- yum install perl
- 脚本安装mysql
- vi /etc/profile
  配置mysql的环境变量
  export PATH=$PATH:/usr/local/mysql/bin
  source /etc/profile
- 启动mysql服务，root用户执行
  service mysql start
- 修改mysql的root用户的密码，登录mysql后
  set password for root@localhost = password('123');
- 修改远程登录的权限，
  use mysql;
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' identified by '123' WITH GRANT OPTION;
  FLUSH PRIVILEGES;
- 关闭centos的防火墙，root用户执行
  service iptables stop
