maven使用指南

## 一、什么是maven

构建：编译、运行单元测试，生成文档，打包和部署，maven的用途之一是服务于构建，通过简单的命令取代重复而繁杂的手工操作。

### 1、maven的安装目录

bin：包含了mvn运行的脚本，该目录还包含了m2.conf文件，这是classworlds的配置文件。

boot：该目录只包含了一个文件plexus-classworlds-xx.jar，这是一个类加载器框架，maven使用该框架加载自己的类库。

conf：该目录包含了一个非常重要的文件settings.xml。直接修改该文件，就能在机器上全局地定制Maven的行为。一般情况下，我们更偏向于复制该文件至～/.m2/目录下（～表示用户目录），然后修改该文件，在用户范围定制Maven的行为。

lib：该目录包含了所有Maven运行时需要的Java类库，Maven本身是分模块开发的，因此用户能看到诸如maven-core-3.0.jar、maven-model-3.0.jar之类的文件。此外，这里还包含一些Maven用到的第三方依赖，如common-cli-1.2.jar、google-collection-1.0.jar等。google-collection-1.0.jar等。

```
mvn help:system    ##打印所以java系统变量和环境变量
```

### 2、设置HTTP代理

对于许多因为安全等方面因素考虑而不能随意访问外网的局域网PC则无法访apache的中央仓库，这就需要在局域网内配置一台代理服务器，需要为Maven配置HTTP代理，才能让它正常访问外部仓库，以下载所需要的资源。

首先需要检测本地网络能不能访问Maven远程仓库，命令为ping repo1.maven.org

接着，要检查代理服务器是否畅通，比如现在有一个IP地址为192.168.10.117，端口为3267的代理服务，需要先运行

telnet  192.168.10.117 3267

来检查该地址的该端口是否畅通，如果得道出错信息需要先获取正确的代理服务器信息，如果telnet连接正确，则输入ctrl+]，然后q，回车，退出即可。

检查完毕之后，编辑~/.m2/settings.xml文件(如果没有该文件，则复制$M2_HOME/conf/settings.xml)。找proxies这个节点,添加代理配置如下：

```
<settings>
  ...
  <proxies>
 proxy>
     <!-- 唯一标识 -->
     <id>my-proxy</id>
     <!-- 指这个代理是否被激活，如果有多个代理，则会寻找第一个被激活的代理 -->
     <active>true</active>
     <!-- 指使用http协议访问 -->
     <protocol>http</protocol>
     <!-- 服务器的IP地址 -->
     <host>218.14.227.197</host>
     <!-- 访问的端口号 -->
     <port>3128</port>
     <!--
         <!-- 如果服务器需要验证，那么这里就需要配置用户名和密码  -->
       <username>***</username>
       <password>***</password>
        <!-- 这里制定那些域名不需要被代理 -->
       <nonProxyHosts>repository.mycom.com|*.google.com</nonProxyHosts>
     -->
   </proxy>	
  </proxies>
  ...
</settings>
```

