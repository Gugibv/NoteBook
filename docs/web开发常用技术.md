环境准备:

安装jdk:下载jdk1.7解压以后,编辑sudo vi /etc/profile.d/jdk.sh添加

```
export JAVA_HOME=/opt/jdk1.7.0_80
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

如果是jdk11就添加下面

```
export JAVA_HOME=/opt/jdk-11.0.1 
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

执行java -vesrion或者javac -version可以看到版本,但是tomcat似乎不支持jdk11,启动tomcat报错,于是降低版本到jdk1.7.

## 一.MyBatis

原名iBatis.数据访问层框架,底层是对JDBC的封装.[官方文档](http://www.mybatis.org/mybatis-3/zh/index.html)

## 二.数据库连接池

在内存中开辟一块空间,存放多个数据库连接对象.

JDBC Tomcat Pool,直接由tomcat产生数据库连接池.

在高频率访问数据库时,使用数据库连接池可以降低服务器系统压力,提升程序运行效率.
