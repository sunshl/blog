## Server Restart

### 服务器

几年前就买了[DigitalOcean](www.digitalocean.com)的服务器，一直用来作为翻墙代理服务器，有点浪费。

最近想要开始写东西，就想好好利用起来，自己搭个博客。（github不便被搜索引擎收录。）

过程中遇到一些问题，备忘一下。

#### SSH登录验证

在DigitalOcean的后台设置了本地SSH的公钥信息，但是还是无法直接登录上。那个功能似乎有些问题。

后来还是通过DigitalOcean的控制台使用用户名密码登录上，然后将本地公钥信息写入到`.ssh/authorized_keys`中，才能直接登录成功。

#### 重装`shadowsocks`

* 通过pip安装，很方便

```
sudo apt-get install python-pip
sudo pip install shadowsocks
/// https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E
```

* 配置及启动

在etc目录下新建了shadowsocks目录，并在该目录下新建`config.json`文件

```
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":8388,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
/// https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File
/// 使用vi拷贝时可能遇到格式不对齐问题，直接通过v命令选中全文后，`==`即可对齐。
```

然后直接通过命令启动

```
sudo ssserver -c /etc/shadowsocks/config.json -d start
```

* 查看日志

```
tail -f /var/log/shadowsocks.log
/// 尝试连接下，看是否输出日志。
```

* 开机启动

在`/etc/rc.local`中增加：

```
sudo /usr/local/bin/ssserver -c /etc/shadowsocks/config.json -d start
```


