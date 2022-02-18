### logback启动远程日志同步步骤：

### 一、本机配置

#### 1.修改文件

vi /etc/rsyslog.conf

```
$ModLoad imudp
$UDPServerRun 514
```

#### 2.重启服务：

````
service rsyslog restart
````

#### 3.开放服务器端口

```
firewall-cmd --zone=public --add-port=514/udp --permanent
```

默认514，可以更改

#### 4.配置logback.xml

```
<appender name="syslog" class="ch.qos.logback.classic.net.SyslogAppender">
        <syslogHost>192.168.6.162</syslogHost>
        <facility>local2</facility>
        <port>514</port>
        <suffixPattern>%-4relative [%thread] %-5level - %msg</suffixPattern>
</appender>
    
<root level="info">
 <appender-ref ref="syslog" />
</root>
```



### 二、远程日志服务器配置

### 一、本机配置

#### 1.修改文件

vi /etc/rsyslog.conf

```
$ModLoad imudp
$UDPServerRun 514

#在配置中新增（日志存储的路径）
local2.info /var/log/local2_info.log
local2.debug /var/log/local2_debug.log
```

#### 2.重启服务：

````
service rsyslog restart
````

#### 3.开放服务器端口

```
firewall-cmd --zone=public --add-port=514/udp --permanent
```

两边端口一致



