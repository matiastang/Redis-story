# Redis配置

* 在redis目录下建立bin，etc，db三个目录
```
sudo mkdir  /usr/local/redis-3.2.8/bin
sudo mkdir  /usr/local/redis-3.2.8/etc
sudo mkdir  /usr/local/redis-3.2.8/db
```
把/usr/local/redis/src目录下的mkreleasehdr.sh，redis-benchmark， redis-check-rdb， redis-cli， redis-server拷贝到bin目录
```
cp /usr/local/redis-3.2.8/src/mkreleasehdr.sh .
cp /usr/local/redis-3.2.8/src/redis-benchmark .
cp /usr/local/redis-3.2.8/src/redis-check-rdb .
cp /usr/local/redis-3.2.8/src/redis-cli .
cp /usr/local/redis-3.2.8/src/redis-server .
```
* 拷贝 redis.conf 到 /usr/local/redis/etc下
  
```
cp /usr/local/redis-3.2.8/redis.conf /usr/local/redis-3.2.8/etc
```
修改redis.conf
```
#修改为守护模式(配置后台运行)
daemonize yes
#设置进程锁文件
pidfile /usr/local/redis-3.2.8/redis.pid
#端口
port 6379
#客户端超时时间
timeout 300
#日志级别
loglevel debug
#日志文件位置
logfile /usr/local/redis-3.2.8/log-redis.log
#设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
databases 16
##指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
#save <seconds> <changes>
#Redis默认配置文件中提供了三个条件：
save 900 1
save 300 10
save 60 10000
#指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，
#可以关闭该#选项，但会导致数据库文件变的巨大
rdbcompression yes
#指定本地数据库文件名
dbfilename dump.rdb
#指定本地数据库路径(默认没有db文件夹，需要创建)
dir /usr/local/redis-3.2.8/db/
#指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能
#会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有
#的数据会在一段时间内只存在于内存中
appendonly no
#指定更新日志条件，共有3个可选值：
#no：表示等操作系统进行数据缓存同步到磁盘（快）
#always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
#everysec：表示每秒同步一次（折衷，默认值）
appendfsync everysec
```
查看配置`cat redis.conf | grep -v "#" | grep -v "^$"`
`cat /usr/local/etc/redis.conf | grep -v "#" | grep -v "^$"`

启动服务
```
./bin/redis-server etc/redis.conf
```
查看日志
```
tail -f log-redis.log
```
打开redis客户端
```
./bin/redis-cli
```

开机启动
brew services start redis

配置文件
/usr/local/etc/redis.conf

编辑配置文件 配置后台运行
daemonize yes

服务端启动及查看
redis-server /usr/local/etc/redis.conf

服务端查看
ps aux | grep redis

客户端调用
redis-cli

给redis设置密码
打开redis.conf文件，找到以下配置项
# requirepass foobared
更改为
requirepass 你的密码
注意：由于上诉操作更改了redis.conf文件，所以下次再启动的时候，要手动加载一下redis.conf文件。例如
进入到redis根目录，启动如下
redis-server /usr/local/etc/redis.conf


 redis没有实现访问控制这个功能，但是它提供了一个轻量级的认证方式，可以编辑redis.conf配置来启用认证。

   1、初始化Redis密码：

   在配置文件中有个参数： requirepass  这个就是配置redis访问密码的参数；

   比如 requirepass test123；

   （Ps:需重启Redis才能生效）

   redis的查询速度是非常快的，外部用户一秒内可以尝试多大150K个密码；所以密码要尽量长（对于DBA 没有必要必须记住密码）；

   2、不重启Redis设置密码：

   在配置文件中配置requirepass的密码（当redis重启时密码依然有效）。

   redis 127.0.0.1:6379> config set requirepass test123

   查询密码：

   redis 127.0.0.1:6379> config get requirepass
   (error) ERR operation not permitted

   密码验证：

   redis 127.0.0.1:6379> auth test123
   OK

   再次查询：

   redis 127.0.0.1:6379> config get requirepass
   1) "requirepass"
   2) "test123"

   PS：如果配置文件中没添加密码 那么redis重启后，密码失效；

   3、登陆有密码的Redis：

   在登录的时候的时候输入密码：

   redis-cli -p 6379 -a test123

   先登陆后验证：

   redis-cli -p 6379

   redis 127.0.0.1:6379> auth test123
   OK

   AUTH命令跟其他redis命令一样，是没有加密的；阻止不了攻击者在网络上窃取你的密码；

   认证层的目标是提供多一层的保护。如果防火墙或者用来保护redis的系统防御外部攻击失败的话，外部用户如果没有通过密码认证还是无法访问redis的。