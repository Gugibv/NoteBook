### 1.ls 列出目录内容

- -l 详细内容(权限,inode,属主,属组,大小,修改时间)
- -a 包括隐藏的所有内容
- -d 只显示目录自身信息
- -i  显示inode信息
- -S 按照文件大小排序
- -r  倒序排序
- -t 按修改时间排序
- -h 文件大小以便于人阅读的方式显示

* ll 相当于 ls -la

### 2.cp 拷贝

- cp file1 file2
- cp file1 file2 dir/   拷贝多个文件到dir目录
- -R/r 拷贝目录及其中的全部内容
- -l 硬链接拷贝 (ls -li) 相当于多起了一个名称,原数据并没有增加
- -s 软链接拷贝 相当于windows的快捷方式,原名称不在了,那么快捷方式也会失效
- -S 目标名称后添加后缀 例如 cp -S 20180106 b b1 目标文件b2后都会添加该后缀
- -u 源比目标文件新时才拷贝 

### 3.mv 移动

- mv file1 file2 
- mv fie1 file2 dir/ 复制文件到dir目录
- -f 强制移动,覆盖目标

### 4.touch 创建空文件

- 若文件名存在,修改文件mtime,但不修改内容
- touch file

### 5.rm 删除

- rm filename
- rm -rf dir/
- -i 每删除前提醒
- -d 删除空文件

### 6.echo 将命令参数显示在stdout

- ehco hello world
- -n 显示结束不换行
- -e 解释执行转义符
- echo $HOME 

### 7.mkdir

- mkdir dir 
- -p 按需创建父目录

### 8.通配符

- echo \*a\*
- ls \*b\*
- ls a?

### 9.grep 

- grep root /etc/passwd  快速筛选文件里面的字符串root
- grep r* /etc/passwd
- grep root /etc/*
- -i 忽略大小写
- -v 反向匹配 不包含该字符串的
- -n 显示行号
- -r 递归目录及目录中所有文件
- -c 显示目标文件中包含关键字的行数

### 10.less

- 每次一页显示输出内容
  - more 的增强版
  - less /usr/share/dict/words
- 快捷键
  - z/b 向前/后翻一页
  - v 进入编辑模式
  - g/G 直接调到第一行/最后一行
  - ./word 向前搜索关键字
  - ?/word 向后搜索关键字
  - n/N 正向/反向继续搜索关键字
  - q 退出
- grep usr/share/dict/words  |  less

### 11.cat 

- cat file1 file2 将两个文件合并显示

### 12.head/tail

- 显示文件头部/尾部内容
- 默认显示10行
- -n n是指定显示的行数数值

### 13.diff

- 比较文本文件
- diff a b
  - -u 统一格式输出
  - -y 并排输出比较
  - -w 忽略空格
  - -i 忽略大小写
- diff a/ b/   比较文件夹

### 14.file

- 检测文件格式

### 15.locate/find

- locate 基于文件索引进行搜索
  - 不验证文件是否存在,速度快但是不准确
  - updatedb 更新索引
- find / -name abc*.txt
- find .-type b c d f l 
- find .-user jing
- find .-mtime +1 -mtime -20

### 16.sort

- sort a.txt
- -r 反向排序
- -M 按月份排序

### 17.vim

- ctrl + b 左
- ctrl + f 右
- ctrl + p 上
- ctrl + n 下
- ctrl + a 光标至行首
- ctrl + e 光标至行尾
- ctrl + w 删除光标前以空格分隔段落
- ctrl + u 删除光标到行首
- ctrl + k 删除光标到行尾
- ctrl + y 黏贴删除的内容  

### 18.vi 文本编辑

- a   在当前光标后添加文本          A 在行末添加文本
- i    在当前光标前插入文本           I  行首插入文本
- o   在光标当前行后插入一空行  O 在光标当前行前插入一空行
- ctrl + f  向文件尾翻一屏
- ctrl + b 向文件首翻一屏
- ZZ    保存当前修改并退出
- D      删除光标到行尾内容
- dd     删除光标所在行全部内容
- ndd   从光标所在行向后删除n行(包括当前行)
- nyy    从光标所在行向下复制n行内容
- p       黏贴
- /字符串   向前搜索关键字
- ? name   向后搜索关键字

### 19.man 

- man 关键词
- man -k keyword 搜关键词(不知道具体命令)

### 20.info

- 操作类似于man

### 21.输出重定向

- ls a/ > file 输出文件夹a的内容到file
- ls a/ >> file 追加文件a的内容到file

### 22.进程管理

- 进程就是运行中的程序文件
  - pid :进程ID
  - TTY: 进行进程的终端设备
  - STAT:进程状态(sleep,running)
  - TIME:该进程占用的CPU时间
  - COMMAND:命令名称
- ps
  - -x :当前用户启动的所有进程
  - -ax:所有用户启动的进程  ps axu 进程显示用户  ps axuw 显示进程的完整路径
  - -ef:打印出全部进程的, 进程名称以及pid
  - ps -ef | grep mysqld :过滤出指定的进程名称
  - -w:显示进程文件完整路径
  - ps -L PID 查看进程的线程
  - pstree 以树状图的方式展现进程之间的派生关系
- 结束进程
  - kill pid 
  - kill -stop pid 
  - kill -KILL pid 或者kill -9 pid 强制结束进程

### 23.归档打包和压缩

- gzip (GNU Zip)

  - linux标准压缩程序 gzip file
  - 解压 gunzip file.gz
  - Gzip不做多文件或目录的归档打包

- tar打包归档(Archive)










