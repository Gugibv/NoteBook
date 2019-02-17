[原文来源](https://blog.csdn.net/m0_37536414/article/details/84850078)

## 配置SSR客户端

Shadowsocks与ShadowsocksR的区别：
SS是linux官方推出的网络代理原版，SSR是原版基础上衍生出来的第三方版本，兼容原版协议，但是比原版多了很多参数设置，即伪装功能（协议和混淆）。
SSR的使用更方便，设置参数等操作更灵活，增加了一些人性化功能，比如服务连接统计、连接管理、协议转换、多重代理等。

1. 下载ssr客户端
   在github上clone客户端工程到当前目录，完成之后当前目录下会有一个叫shadowsocksr的文件夹
   `git clone https://github.com/ssrbackup/shadowsocksr`

2. 配置ss文件
   进入刚刚clone下来的文件夹，有一个文件叫`config.json`，这是配置文件的模板，里面就是我们需要设置的代理参数，复制一份到`/etc/shadowsocks.json`:

   `cp config.json /etc/shadowsocks.json`

   然后对这个文件进行配置：

   `sudo gedit /etc/shadowsocks.json`

   打开文件之后，我们看到的具体内容就包含了我们需要配置代理的详细参数，具体情况如下图所示：

```
{
    "server": "0.0.0.0",
    "server_ipv6": "::",
    "server_port": 8388,
    "local_address": "127.0.0.1",
    "local_port": 1080,

    "password": "m",
    "method": "aes-128-ctr",
    "protocol": "auth_aes128_md5",
    "protocol_param": "",
    "obfs": "tls1.2_ticket_auth_compatible",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {}, // only works under multi-user mode
    "additional_ports_only" : false, // only works under multi-user mode
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",
    "fast_open": false
}
12345678910111213141516171819202122232425
```

主要用到的配置是下面几个选项，但是具体配置哪些参数还得看SS供应商给的参数情况，尽量多填但是要保证正确，如果有错的话代理是不能通过的：

```
 	"server": "0.0.0.0",		//服务器ip地址
 	"server_port": 8388，		 //服务器端口
 	"password": "m",			//服务器密码
 	"method": "aes-128-ctr",	//加密方式
 	"protocol": "auth_aes128_md5",		//协议
 	"protocol_param": "",					//协议参数
 	"obfs": "tls1.2_ticket_auth_compatible",		//混淆
 	"obfs_param": "",								//混淆参数
12345678
```

3.启动ssr客户端：

```
python ~/shadowsocksr/shadowsocks/local.py -c /etc/shadowsocks.json        
1
```

注：如果出错：

```
IPv6 support
Traceback (most recent call last):
  File "local.py", line 81, in <module>
    main()
  File "local.py", line 43, in main
    config = shell.get_config(True)
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/shell.py", line 299, in get_config
    check_config(config, is_local)
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/shell.py", line 129, in check_config
    encrypt.try_cipher(config['password'], config['method'])
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/encrypt.py", line 46, in try_cipher
    Encryptor(key, method)
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/encrypt.py", line 90, in __init__
    random_string(self._method_info[1]))
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/encrypt.py", line 119, in get_cipher
    return m[2](method, key, iv, op)
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/crypto/sodium.py", line 71, in __init__
    load_libsodium()
  File "/home/miliimoulins/shadowsocksr/shadowsocks/../shadowsocks/crypto/sodium.py", line 42, in load_libsodium
    raise Exception('libsodium not found')
Exception: libsodium not found
123456789101112131415161718192021
```

可能因为当前python版本为系统自带的python2.7，却很多东西，将python默认版本改为python3就可以，具体修改方法可以参考

[Ubuntu16.04下完美切换Python版本](https://blog.csdn.net/beijiu5854/article/details/77897767)

修改之后再运行上面指令就可以运行成功啦：

```
IPv6 support
2018-12-06 11:11:05 INFO     util.py:85 loading libsodium from libsodium.so.23
2018-12-06 11:11:05 INFO     local.py:50 local start with protocol[auth_aes128_md5] password [NAUgcj] method [chacha20] obfs [http_post] obfs_param [tvb.com]
2018-12-06 11:11:05 INFO     local.py:54 starting local at 127.0.0.1:1080
2018-12-06 11:11:05 INFO     asyncdns.py:324 dns server: [('127.0.0.53', 53)]

123456
```

1. 转换HTTP代理
   shadowsocks默认是用socks5协议的，对于终端的get、wget等走的是http协议的地方是[无能为力](https://www.baidu.com/s?wd=%E6%97%A0%E8%83%BD%E4%B8%BA%E5%8A%9B&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)的，所以需要转换成http代理，加强通用性，这里使用的转换方法是基于Polipo的。

```
sudo apt-get install polipo      # 安装Polipo
sudo gedit /etc/polipo/config    # 修改配置文件
12
```

将下面的内容整个替换到文件中并保存：

```
# This file only needs to list configuration variables that deviate
# from the default values. See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.
logSyslog = false
logFile = "/var/log/polipo/polipo.log"

socksParentProxy = "127.0.0.1:1080"
socksProxyType = socks5

chunkHighMark = 50331648
objectHighMark = 16384

serverMaxSlots = 64
serverSlots = 16
serverSlots1 = 32

proxyAddress = "0.0.0.0"
proxyPort = 8123
123456789101112131415161718
```

重启Polipo:

```
/etc/init.d/polipo restart
1
```

验证代理是否正常工作：

```
export http_proxy="http://127.0.0.1:8123/"
curl www.google.com
12
```

如果正常工作的话，就会返回[一大](https://www.baidu.com/s?wd=%E4%B8%80%E5%A4%A7&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)串东西，其实就是从Google网页上抓下来的内容，这样终端里面也可以访问外网了。另外，在浏览器中输入<http://127.0.0.1:8123/>便可以进入到Polipo的使用说明和配置界面。

1. 配置浏览器
   无论对于firefox或者chrome，有的配置方法对与firefox管用，但是对于chrome却不管用，所以这里我们直接来修改全局网络，对于firefox和chrome都适用。
   打开`setting -->Network -->Network Proxy` : `Method:Manual`
   下面的几栏都设置为：`127.0.0.1` 端口：`8123` 保存或者应用到全局（`Apply system wide`）
2. 至此，恭喜你，所有该设置的都已经设置完成了。

## 注意

每次开机后都要执行：`python ~/shadowsocksr/shadowsocks/local.py -c /etc/shadowsocks.json`来启动ss服务。而且如果直接ctrl+c关掉进程或者关闭当前终端都会直接让网络挂掉，百度网都上不上去，重启电脑也没用，所以处理方法是：

## Solution:

1. 首先一般这种情况下，如果你不是直接关闭的当前terminal，则你的Python进程可能还没有杀掉，所以 `ps aux|grep python`，然后`kill -9 process_number`（那个python进程的process number）。
2. 然后重新启动ss服务。ps：**我建议用screen命令来执行**，至少不用担心误杀进程：`screen python ~/shadowsocksr/shadowsocks/local.py -c /etc/shadowsocks.json`。pps：screen模式下，ctrl+a，然后d（detach），或者直接可以关闭当前terminal。
3. **如果还是不能上网**： 重启polipo：`/etc/init.d/polipo restart`。
4. 验证：`export http_proxy="http://127.0.0.1:8123/"`，`curl www.google.com`。

## 更方便的处理方法，写个脚本：

我们可以在比如/home下新建个文件叫做shadow.sh，在里面写上我们启动ssr客户端需要的命令，然后保存即可。因为有时候会遇到启动ss服务后依旧内网外网都不能上，就每次都自动重启一下polipo吧：

```
#！/bin/bash
#shadow.sh
screen python ~/shadowsocksr/shadowsocks/local.py -c /etc/shadowsocks.json
/etc/init.d/polipo restart
1234
```

到终端执行命令 `sh /home/shadow.sh`，成功的话会有信息输出。你也可以到浏览器去试试。

**至此**：开机后打开终端运行：`sh /home/shadow.sh`，就可以自由上网了。

