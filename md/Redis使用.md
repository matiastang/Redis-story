<!--
 * @Descripttion: 
 * @version: 
 * @Author: sueRimn
 * @Date: 2020-08-17 18:29:49
 * @LastEditors: sueRimn
 * @LastEditTime: 2020-08-17 18:39:30
-->
# Redis使用

关闭服务端

强行关闭
强行终止redis进程可能会导致数据丢失，因为redis可能正在将内存数据同步到硬盘中。
 ps axu|grep redis  ## 查找redis-server的PID
 kill -9 PID

 今天在研究redis集群的时候发现了一个redis的快照缓存机制。
如果通过kill -9 命令删除的redis进程,是不会保存数据到快照文件的。

命令关闭
向redis发送SHUTDOWN命令，即 redis-cli SHUTDOWN 。Redis收到命令后，服务端会断开所有客户端的连接，然后根据配置执行持久化，最后退出。
## 启动redis-server，后台线程
AT8775:redis shoren$ redis-server /usr/local/redis/etc/redis.conf 
## 启动成功
AT8775:redis shoren$ ps axu|grep redis
shoren           14948   0.0  0.0  2434840    760 s000  S+   10:18上午   0:00.00 grep redis
shoren           14946   0.0  0.0  2452968   1492   ??  Ss   10:18上午   0:00.01 redis-server *:6379 
## 关闭服务器
AT8775:redis shoren$ redis-cli shutdown

redis 设置密码登录后，想关闭redis服务器，需要

redis-cli -a 密码 shutdown 

## 关闭成功
AT8775:redis shoren$ ps axu|grep redis
shoren           14952   0.0  0.0  2435864    772 s000  S+   10:19上午   0:00.01 grep redis

启动客户端
默认启动
使用命令redis-cli启动客户端，按照默认配置连接Redis（127.0.0.1:6379）。

指定地址和端口号
使用命令 redis-cli -h 127.0.0.1 -p 6379

关闭客户端
交互模式使用quit

AT8775:redis shoren$ redis-cli -h 127.0.0.1 -p 6379
## 简单使用set、get命令
127.0.0.1:6379> set key value12
OK
127.0.0.1:6379> get key
"value12"
## 退出
127.0.0.1:6379> quit
AT8775:redis shoren$ 

更改log-redis.log文件的权限
chmod 777 log-redis.log

conf文件 修改数据本地保存文件配置

#指定本地数据库文件名
dbfilename dump.rdb
#指定本地数据库路径 默认是当前路径。<br>dir ./　　# 当前路径（配置文件所在路径） chmod 777 后,redis可以成功shutdown，有dump.rdb文件
# dir /usr/local/redis/db/ # 设置的其它路径chmod 777 后,redis依然无法shutdown,这个路径下也无法查找到dump.rdb文件

redis常用命令
命令	用途
set key value	设置 key 的值
get key	获取 key 的值
exists key	查看此 key 是否存在
keys *	查看所有的 key
flushall	消除所有的 key