## 一.MyBatis

原名iBatis.数据访问层框架,底层是对JDBC的封装.[官方文档](http://www.mybatis.org/mybatis-3/zh/index.html)

## 二.数据库连接池

在内存中开辟一块空间,存放多个数据库连接对象.

JDBC Tomcat Pool,直接由tomcat产生数据库连接池.

在高频率访问数据库时,使用数据库连接池可以降低服务器系统压力,提升程序运行效率.
