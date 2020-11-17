#### 基础命令

```c
topeet@ubuntu：~$
```

==@符号==前面的topeet 是当前登录的用户名称，@符号后面的ubuntu 是ubuntu 系统的名称，最后面的==$符==
==号==表示当前是普通用户。

##### 文件信息查看 ls

```c
ls [选项] [路径]
    
	-a 显示所有的文件以及子目录，包括以“.”开头的隐藏文件
	-l 显示文件的详细信息，比如文件的形态、权限、所有者、大小等信息
	-t 将文件按照创建时间排序列出
	-s 在每个文件的后面打印出文件的块大小
	-A 列出除了"."和".."以外的文件
	-R 将目录下所有的子目录的文件都列出来，相当于我们编程中的“递归”实现
	-L 列出文件的链接名。Link（链接）
	-S 以文件的大小进行排序
```

##### 显示当前路径命令pwd

```c
pwd [OPTION]...
    
    -L 目录连接链接时，输出连接路径
	-P 输出物理路径
```

##### 系统信息查看命令uname

```c
uname [-amnrsv] [--help] [--version]
    
    -a //显示全部的信息
    -m //显示电脑类型
    -n //显示在网络上的主机名称
    -r //显示操作系统的发行编号
    -s //显示操作系统名称
    -v //显示操作系统的版本
    --help //显示帮助
    --version //显示版本信息
```

##### 切换用户执行身份命令sudo

```c
sudo [选项] [命令]
```

##### 添加\删除用户命令adduser\deluser

```c
adduser [参数] [用户名]
    
    -system //添加一个系统用户
    -home DIR //DIR 表示用户的主目录
    -uid ID //ID 表示用户的uid
    -ingroup GRP //表示用户所属的组名
    
deluser[参数] [用户名]
    
    -system //当用户是一个系统用户的时候才能删掉
    -remove-home //删除用户的主目录
    -remove-all-files //删除与用户有关的所有文件
    -backup //备份用户信息
```

##### 切换用户命令su

```c
su [选项] [用户名]
    
    -c command 执行指定的命令，执行完毕以后在回到原用户身份
    -l 改变用户身份，同时改变工作目录和系统的环境变量
    -m 改变用户身份，不改变系统环境变量
    -h 显示帮助信息
```

##### 查看文件内容命令cat

```c
cat [选项] [文件]
    -n 从1 开始，对输出的行进行编号
    -b 对输出的行进行编号（空白行除外）
    -s 当遇到连续的两个空白行时，合并成一个空白行
```

##### 网络配置命令ifconfig

```c
ifconfig interface options| address
```

##### 帮助命令man

```C
man [命令名称]
```

##### 系统重启命令reboot

##### 系统关机命令poweroff

#### apt-get

##### 更新软件列表

```c
sudo apt-get update
```

##### 检查依赖是否有损坏

```c
sudo apt-get check
```

##### 软件安装

```c
sudo apt-get install package-name
```

##### 软件更新

```c
sudo apt-get upgrade
```

##### 软件卸载

```c
sudo apt-get remove package-name
```



