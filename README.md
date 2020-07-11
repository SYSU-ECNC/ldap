# ldap
在Ubuntu18.04快速部署ldap服务


## **步骤**

1、安装ladp

```
sudo apt-get install -y slapd ldap-utils
```

2、配置/etc/ldap/ldap.conf, 添加BASE 和 URI. 这里的BASE为dc=ldapdomain,dc=com  URI为ldap://\<ip>:389

```
BASE     dc=ecnc,dc=com
URI      ldap://<ip>:389
```


如果重新配置ldap：

```
dpkg-reconfigure slapd
```


3、安装php的ldap管理端软件：

```
apt-get install -y phpldapadmin
```

修改相应的配置文件/etc/phpldapadmin/config.php，做如下修改：

```
(1) $servers->setValue('server'. 'host', '127.0.0.1')#修改为某个内网可访问的IP地址

(2) $servers->setValue('server'. 'base', array('dc=example,dc=com')) #修改为baseDN，这里修改为dc=ecnc,dc=com

(3) $servers->setValue('login', 'bind_id', 'cn=admin,dc=example,dc=com')#修改为baseDN下的admin, cn=admin,dc=ecnc,dc=com

(4) $config->custom->appearance['hide_template_warning'] = false #false修改为true
```

4、防火墙放行

```
ufw allow "Apache"
ufw allow "Apache Full"
ufw allow "Apache Secure" 
```

5、替换/usr/share/phpldapadmin/lib/functions.php

```
# 因为php7.2不支持create_function()
下载链接：
http://ealeofile.ealeo.xyz/f/af33d990121b4649a436/?dl=1
```
6、重启服务
```
/etc/init.d/apache2 restart
```

