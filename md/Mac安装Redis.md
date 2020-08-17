# Mac安装Redis

[官网](https://redis.io)
[Redis客户端(收费)](https://redisdesktop.com/)
[Redis客户端(免费，自己编译)](https://github.com/uglide/RedisDesktopManager/releases)

1. [官网下载](https://redis.io/download)Redis（建议下载最新的稳定版本）
2. 解压，下载压缩包目录，命令行执行`tar zxvf redis-6.0.6.tar.gz`
3. 移动文件，`mv redis-6.0.6 /usr/local/`
4. 进入`redis-6.0.0`目录
5. 编译测试 `sudo make test`
6. 编译安装 `sudo make install`
7. 查看安装的版本
```
redis-server -v
redis-cli -v
```
* 启动server`redis-server`
* 启动cli`redis-cli`

