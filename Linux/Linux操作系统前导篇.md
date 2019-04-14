[TOC]

- [Linux操作系统前导课程](#Linux操作系统前导课程)

  - [sudo](#sudo)

  - [使用apt-get管理软件](#使用apt-get管理软件)

  - [使用dpkg安装软件包](#使用dpkg安装软件包)

  - [使用工具安装软件](#使用工具安装软件)

  - [精简的Linux手册](#精简的Linux手册)

  - [免密登录](#免密登录)

  - [基本命令](#基本命令)
    - [cd](#cd)
    - [pwd](#pwd)
    - [ls](#ls)
    - [mkdir](#mkdir)
    - [rm](#rm)
    - [cp](#cp)
    - [cat](#cat)
    - [echo](#echo)
    - [touch](#touch)
    - [mv](#mv)

  - [命令名](#命令名)

    

# Linux操作系统前导课程

## sudo

> ```c
> //ssh zhengdongqi@192.168.1.44
> //pi5
> //寻找ip地址 ifconfig
> //Mac-ip 192.168.2.33
> //pi5 
> //ssh-copy-id -i ~/.ssh/id_rsa.pub user@192.168.1.40 一键免密
> //本地->pi
> //scp 路径 user@pi5 路径
> //pi->本地
> //scp 路径 zhengdongqi@192.168.2.165:~
> //scp zhengdongqi@192.168.1.44:~/0223.zip ./
> //修改IP重命名 vim /etc/hosts
> //scp -r /Users/zhengdongqi/Project/Socket_Pro zhengdongqi@192.168.1.44:/home/zhengdongqi/
> ```
>
> ```c
> 方案一：在客户端设置
> 方法很简单，只需在客户端电脑上编辑（需要root权限）/etc/ssh/ssh_config，并添加如下一行：
> 
> 1
> ServerAliveInterval 60
> 此后该系统里的用户连接SSH时，每60秒会发一个KeepAlive请求，避免被踢。
> 
> 方案二：在服务器端设置
> 如果有相应的权限，也可以在服务器端设置，即编辑/etc/ssh/sshd_config，并添加：
> 
> 1
> ClientAliveInterval 60
> ```
>
> 



> `sudo`命令一般被用于临时获取管理员权限，形象的，可理解为**Windows**的**以管理员权限运行**。

> ```bash
> sudo reboot #普通用户使用sudo命令临时获取管理员权限并执行重启操作
> sudo -i 	#普通用户切换到root用户
> exit 		#退出当前bash，相应的会退出使用sudo切换的root用户
> logout		#效果等同于exit
> ctrl + d	#使用快捷键效果与exit，logout一致
> ```
>

> 小技巧：在平时使用Linux操作系统的时候，可能会经常发现很长的一段命令输完之后，系统提示自己没有权限，也就是**Permission denied**,遇到这种情况，可以使用直接执行`sudo !!`这个命令，它的作用就是在上一条执行的命令前面直接加一个**sudo**并再次执行。

## 使用apt-get管理软件

> 在学习`apt-get`这个命令之前，我们要先了解Linux**源**的概念。
>
> 不止**Debian**系的Ubuntu等OS有源的概念，**RedHat**系的rhel，cent os，**SUSE**系的opensuse等都有**源**的概念，可以说**源**是Linux操作系统必备的一个概念。
>
> **源解决的其实就是软件之间的依赖关系**，可以直观的把**源**看成一个**软件仓库**，通过配置本地源或者网络源，就可以在Linux上很方便的安装许多软件，而不用关注软件之间的依赖关系。

- ​	在使用apt-get命令前，一般的做法是为自己的OS换源，因为Ubuntu自带的官方源列表，可能在本地访问的时候因为各种因素导致访问的效果并不好，所以需要根据自己的情况，选择合适的网络源。*一般来说，阿里和网易的源速度都不错，如果是校园网用户，可以尝试各大高校的源。*


- ​	**Ubuntu**操作系统的源列表配置文件为**/etc/apt/sources.list**，在网上搜索系统版本对应的源列表，删除原来的信息，写在该文件中即可。


> ```bash
> apt-get update		#更新源信息，该操作将访问所有源列表，并获取最新的软件信息
> apt-get upgrade		#将自己本地的程序与源信息列表的进行比对，并升级为最新版本
> apt-get install A	#安装特定软件，软件名完全匹配
> apt-cache search A	#在apt缓存的软件列表中查找软件，模糊匹配
> apt-get remove A	#删除已安装的软件A，但是保留配置文件
> apt-get --purge remove A #删除软件A，并且删除配置文件
> apt-get autoremove  #删除软件A，并且删除为了满足A的依赖而存在的软件，该命令要慎重使用，初学者不建议用
> apt-get autoclean	#删除apt-get安装软件后，在/var/cache/apt/archives/ 存在的已过期的deb包
> apt-get clean		#删除/var/cache/apt/archives/ 中所有的软件安装包。
> ```
>

## 使用dpkg安装软件包

​	在Linux下安装软件除了使用apt-get之外，还有源码安装，直接使用软件包安装等方法，`dpkg`是**Debian**系操作系统使用的软件包安装方式，使用apt-get的方式，其实底层就是`dpkg`的方式。对于一些使用`dpkg`方法安装的软件，可能会遇到软件依赖关系的问题，并且`deb`安装包只能对应某一特定的操作系统，没有使用源码安装那样有很强的系统适应性。

> ```bash
> dpkg -i a.deb #使用安装包a.deb安装软件
> ```
>

## 使用工具安装软件

> `pip`是一个Python包管理工具，主要是用于安装 `PyPI` 上的软件包。

> ```bash
> apt-get install python-pip		#安装pip2
> apt-get install python3-pip		#安装pip3
> pip install --upgrade pip		#升级pip
> pip install tldr				#安装tldr
> pip uninstall tldr				#卸载tldr
> ```
>



## 精简的Linux手册

> `tldr`取名自“**t**oo **l**ong **d**on't **r**ead ”,是一个**github**上的开源项目，区别于`man`和`info`冗长而详细的文档，它提供了一种简洁的对Linux常用命令的使用手册，虽然不够详尽，但是基本能满足我们日常的需求。

> ```bash
> tldr ls		#使用tldr查看ls命令
> ```
>



## 免密登录

> Linux 免密登录，其实就是使用密钥对的方式代替传统的用户名密码验证方式进行远程登录。

​	要实现Linux的免密登录，假设A是本地机，B为要实现远程登录的机器，需要完成以下几个步骤：

1. 在主机A上使用`ssh-keygen`命令创建密钥对；
2. 在主机B上，在要实现免密登录的用户的家目录下，创建`.ssh`文件夹（如果已存在不用操作）；
3. 使用`scp`命令将主机A上自己的公钥(`~/.ssh/id_rsa.pub`)拷贝到目标主机B的`.ssh`文件夹下，并追加到authorized_keys文件中(没有可自己添加)。

## 基本命令

### cd

> ```bash
> cd /etc		#直接切换到/etc目录
> cd ..		#切换到上层目录
> cd .		#切换到当前目录
> cd 			#回到自己的家目录
> cd ~		#回到自己的家目录
> cd -		#回到上次工作目录
> ```
>

> 1. 并不是每个用户的家目录都是在形如`/home/username/`的目录下，但是大多数情况下是这样的。可以使用命令`echo $HOME`来查看当前用户的家目录。家目录是在用户创建的时候被定义的，当然我们也可以自己修改。
> 2. `cd`命令是最常见的Linux命令，它用以切换我们的工作目录，初学者在使用Linux的时候，一定要注意自己当前的工作目录。

### pwd

​	`pwd`意为“**P**rint **W**orking **D**iretory”,打印当前工作目录，在终端中执行`pwd`可以直接在屏幕上打印自己的工作目录。

> ```bash
> suyelu@HaiZei-Tech:/etc/apt$ pwd			#打印当前个工作目录
> /etc/apt
> suyelu@HaiZei-Tech:/etc/apt$ dir=`pwd`		#将命令pwd的结果赋值给变量dir
> suyelu@HaiZei-Tech:/etc/apt$ cd				#回到家目录
> suyelu@HaiZei-Tech:~$ cd $dir				#$dir的含义是去变量的值，使用cd 命令切换到以前的路径
> suyelu@HaiZei-Tech:/etc/apt$ pwd			#打印新的工作目录
> /etc/apt
> ```
>



### ls

​	`ls`意为"**l**i**s**t",列出路径下的文件(*Linux下一切皆文件*，目录也是文件)。

> ```bash
> ls				#列出当前工作目录下的所有非隐藏文件
> ls -a			#列出当前工作目录下的所有文件
> ls -al			#以列表的形式列出当前目录下的所有文件
> ll				#其实是ls -al的别名，Linux中并没有这个命令
> ls /etc			#列出/etc目录下的
> ```
>



### mkdir

​	`mkdir`意为“**m**a**k**e **dir**ectory”,创建目录。

> ```bash
> mkdir haizei
> mdkir -p /home/haizei/haizei/haizei
> ```
>



### rm

​	`rm`意为"**r**e**m**ove",删除。

> ```bash
> rm haizei.text
> rm -f haizei.txt
> rm -r haizei
> rm -rf haizei
> ```
>

### cp

​	`cp`意为“**c**o**p**y”，拷贝。

> ```bash
> cp file1 file2
> cp file1 dir
> cp -r dir dir
> cp -a dir dir
> ```
>



### cat

​	`cat`意为“con**cat**enate”，把多个文件连接起来，通常用来读取一个文本文件的内容。它有很多高级用法，在这里不详细讲述，现阶段可以直接使用`cat file`,来直接读取文件内容。

> 除了使用cat可以查看文件内容外，下面的Linux命令也可以查看文本文件的内容。
>
> 1. more ，对于大文件，按页来查看
> 2. less，与more类似，不同的是less可以向上翻页(**这一点在很多面试中出现过**)
> 3. head，默认获取文件的前10行
> 4. tail，与head恰好想反，获取文件的末尾十行

### echo

​	`echo`也是一个很常见的Linux命令，它的作用是向屏幕打印内容，也可以搭配重定向符对文件做一些重写或者追加操作。

### touch

​	`touch`命令的主要作用是用来修改文件的访问时间和修改时间，但是更多的时候，我们使用`touch`命令来新建一个空白文件。

### mv

​	 `mv`，意为“**m**o**v**e”，移动。可以将文件移动到新的目录，或者对文件实现重命名的操作。

> 在Shell下，对文件重命名可以使用`cat`,`cp`,`mv`等多种方式。

# 命令名

> ```c
> hostname  //用以显示或设置系统的主机名称
> pathname //路径名
> pw_name //登陆用户名
> pw_dir //根目录
>     
> passwd //一种计算机命令、文件的名称。passwd命令用来更改使用者的密码，passwd文件通常在Linux系统中，用户的关键信息被存放在系统的/etc/passwd文件中。
> 
> uid_t getuid(void); //定义在头文件sys/types.h中，它通常是一个小整形。函数返回一个调用程序的真实用户ID，一般来说，这个函数都是会调用成功的。头文件 #include<sys/types.h> #include<unistd.h>
> 
> struct passwd *getpwuid(uid_t uid); //函数是通过用户的uid查找用户的passwd数据。如果出错时，它们都返回一个空[指针]并设置errno的值，用户可以根据perror函数查看出错的信息。头文件 #include <sys/types.h> #include <pwd.h>
> 
> char *getcwd( char *buffer, int maxlen );//会将当前工作目录的绝对路径复制到参数buffer所指的内存空间中,参数maxlen为buffer的空间大小。头文件：unistd.h（windows下为direct.h）
> 
> char *strtok(char s[], const char *delim);//strtok()用来将字符串分割成一个个片段。参数s指向欲分割的字符串，参数delim则为分割字符串中包含的所有字符。当strtok()在参数s的字符串中发现参数delim中包含的分割字符时,则会将该字符改为\0 字符。在第一次调用时，strtok()必需给予参数s字符串，往后的调用则将参数s设置成NULL。每次调用成功则返回指向被分割出片段的指针
> 
> struct dirent
> {
> long d_ino; /* inode number 索引节点号 */
> off_t d_off; /* offset to this dirent 在目录文件中的偏移 */
> unsigned short d_reclen; /* length of this d_name 文件名长 */
> unsigned char d_type; /* the type of d_name 文件类型 */
> char d_name [NAME_MAX+1]; /* file name (null-terminated) 文件名，最长256字符 */
> }
> 相关函数
> opendir()，readdir(),closedir();
> 
> 在计算机语言中
> 函数名: stat()
> 功 能: 得到文件的信息，将其保存在buf结构中，buf的地址以参数形式传递给stat。
> 用 法: int _stat(const char *path,struct _stat *buffer)
> 参数:
> const char *path: 文件名或者目录名
> struct _stat *buffer:结构体对象地址
> 返回值: 成功返回0，返回-1表示失败。
> stat 结构定义于：/usr/include/sys/stat.h 文件中
> structstatfinfo;
> stat(sFileName,&finfo);
> intsize=finfo.st_size;
> structstat
> mode_t st_mode;//文件对应的模式，文件，目录，权限等
> ino_t st_ino;//i-node节点号
> dev_t st_dev;//设备号码
> dev_t st_rdev;//特殊设备号码
> nlink_t st_nlink;//文件的硬链接数
> uid_t st_uid;//文件所有者
> gid_t st_gid;//文件所有者对应的组
> off_t st_size;//普通文件，对应的文件字节数
> time_t st_atime;//文件最后被访问的时间
> time_t st_mtime;//文件内容最后被修改的时间
> time_t st_ctime;//文件状态（属性）改变时间
> blksize_t st_blksize;//文件内容对应的块大小
> blkcnt_t st_blocks;//文件内容对应的块数量
> 
> 从文件结构体指针stream中读取数据，每次读取一行。读取的数据保存在buf指向的字符数组中，每次最多读取bufsize-1个字符（第bufsize个字符赋'\0'），如果文件中的该行，不足bufsize-1个字符，则读完该行就结束。如若该行（包括最后一个换行符）的字符数超过bufsize-1，则fgets只返回一个不完整的行，但是，缓冲区总是以NULL字符结尾，对fgets的下一次调用会继续读该行。函数成功将返回buf，失败或读到文件结尾返回NULL。因此我们不能直接通过fgets的返回值来判断函数是否是出错而终止的，应该借助feof函数或者ferror函数来判断。
> 
> passwd结构体
> 在<pwd.h>中定义
> struct passwd
> {
> char * pw_name; /* Username, POSIX.1 */
> char * pw_passwd; /* Password */
> __uid_t pw_uid; /* User ID, POSIX.1 */
> __gid_t pw_gid; /* Group ID, POSIX.1 */
> char * pw_gecos; /* Real Name or Comment field */
> char * pw_dir; /* Home directory, POSIX.1 */
> char * pw_shell; /* Shell Program, POSIX.1 */
> char *pw_class;
> time_t pw_change;
> time_t pw_expire;
> };
> 在POSIX.1中只指定了passwd结构包含的10个字段的五个。大多数平台至少支持其中七个字段。bsd派生的平台支持全部十个字段。
> fork();
> (1)在父进程中，fork返回新创建子进程的进程ID；
> (2)在子进程中，fork返回0；
> (3)如果出现错误，fork返回一个负值。
> SIG_IGN 忽略参数signum指定的信号。
> SIG_DFL 将参数signum 指定的信号重设为核心预设的信号处理方式。
> SIGINT	通过ctrl+c将会对当进程发送此信号	信号被当前进程树接收到，也就是说，不仅当前进程会收到信号，它的子进程也会收到
> SIGTERM	kill命令不加参数就是发送这个信号	只有当前进程收到信号，子进程不会收到。如果当前进程被kill了，那么它的子进程的父进程将会是init，也就是pid为1的进程
> linyb@linyb-QTJ5:~$ kill -l
>  1) SIGHUP   2) SIGINT   3) SIGQUIT  4) SIGILL   5) SIGTRAP
>  6) SIGABRT  7) SIGBUS   8) SIGFPE   9) SIGKILL 10) SIGUSR1
> 11) SIGSEGV 12) SIGUSR2 13) SIGPIPE 14) SIGALRM 15) SIGTERM
> 16) SIGSTKFLT   17) SIGCHLD 18) SIGCONT 19) SIGSTOP 20) SIGTSTP
> 21) SIGTTIN 22) SIGTTOU 23) SIGURG  24) SIGXCPU 25) SIGXFSZ
> 26) SIGVTALRM   27) SIGPROF 28) SIGWINCH    29) SIGIO   30) SIGPWR
> 31) SIGSYS  34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
> 38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
> 43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
> 48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
> 53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
> 58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
> 63) SIGRTMAX-1  64) SIGRTMAX   
> SIGHUP     终止进程     			终端线路挂断
> SIGINT     终止进程     			中断进程
> SIGQUIT    建立CORE文件终止进程，	 并且生成core文件
> SIGILL     建立CORE文件       		非法指令
> SIGTRAP    建立CORE文件      		跟踪自陷
> SIGBUS     建立CORE文件             总线错误
> SIGSEGV    建立CORE文件       		段非法错误
> SIGFPE     建立CORE文件       		浮点异常
> SIGIOT     建立CORE文件       		执行I/O自陷
> SIGKILL    终止进程     			杀死进程
> SIGPIPE    终止进程     			向一个没有读进程的管道写数据
> SIGALARM   终止进程     			计时器到时
> SIGTERM    终止进程     			软件终止信号
> SIGSTOP    停止进程     			非终端来的停止信号
> SIGTSTP    停止进程     			终端来的停止信号
> SIGCONT    忽略信号     			继续执行一个停止的进程
> SIGURG     忽略信号     			I/O紧急信号
> SIGIO      忽略信号     			描述符上可以进行I/O
> SIGCHLD    忽略信号     			当子进程停止或退出时通知父进程
> SIGTTOU    停止进程     			后台进程写终端
> SIGTTIN    停止进程     			后台进程读终端
> SIGXGPU    终止进程     			CPU时限超时
> SIGXFSZ    忽略信号     			窗口大小发生变化
> SIGPROF    终止进程     			统计分布图用计时器到时
> SIGUSR1    终止进程     			用户定义信号1
> SIGUSR2    终止进程     			用户定义信号2
> SIGVTALRM  终止进程     			虚拟计时器到时
> SIGABRT		由调用abort函数产生，进程非正常退出
> SIGALRM		用alarm函数设置的timer超时或setitimer函数设置的interval timer超时
> SIGBUS		某种特定的硬件异常，通常由内存访问引起
> SIGCANCEL	由Solaris Thread Library内部使用，通常不会使用
> SIGCHLD		进程Terminate或Stop的时候，SIGCHLD会发送给它的父进程。缺省情况下该Signal会被忽略
> SIGCONT		当被stop的进程恢复运行的时候，自动发送
> SIGEMT		和实现相关的硬件异常
> SIGFPE		数学相关的异常，如被0除，浮点溢出，等等
> SIGFREEZE	Solaris专用，Hiberate或者Suspended时候发送
> SIGHUP		发送给具有Terminal的Controlling Process，当terminal被disconnect时候发送
> SIGILL		非法指令异常
> SIGINFO		BSD signal。由Status Key产生，通常是CTRL+T。发送给所有Foreground Group的进程
> SIGINT		由Interrupt Key产生，通常是CTRL+C或者DELETE。发送给所有ForeGround Group的进程
> SIGIO		异步IO事件
> SIGIOT		实现相关的硬件异常，一般对应SIGABRT
> SIGKILL		无法处理和忽略。中止某个进程
> SIGLWP		由Solaris Thread Libray内部使用
> SIGPIPE		在reader中止之后写Pipe的时候发送
> SIGPOLL		当某个事件发送给Pollable Device的时候发送
> SIGPROF		Setitimer指定的Profiling Interval Timer所产生
> SIGPWR		和系统相关。和UPS相关。
> SIGQUIT		输入Quit Key的时候（CTRL+\）发送给所有Foreground Group的进程
> SIGSEGV		非法内存访问
> SIGSTKFLT	Linux专用，数学协处理器的栈异常
> SIGSTOP		中止进程。无法处理和忽略。
> SIGSYS		非法系统调用
> SIGTERM		请求中止进程，kill命令缺省发送
> SIGTHAW		Solaris专用，从Suspend恢复时候发送
> SIGTRAP		实现相关的硬件异常。一般是调试异常
> SIGTSTP		Suspend Key，一般是Ctrl+Z。发送给所有Foreground Group的进程
> SIGTTIN		当Background Group的进程尝试读取Terminal的时候发送
> SIGTTOU		当Background Group的进程尝试写Terminal的时候发送
> SIGURG		当out-of-band data接收的时候可能发送
> SIGUSR1	    用户自定义signal 1
> SIGUSR2	    用户自定义signal 2
> SIGVTALRM	setitimer函数设置的Virtual Interval Timer超时的时候
> SIGWAITING	Solaris Thread Library内部实现专用
> SIGWINCH	当Terminal的窗口大小改变的时候，发送给Foreground Group的所有进程
> SIGXCPU	    当CPU时间限制超时的时候
> SIGXFSZ	    进程超过文件大小限制
> SIGXRES	    Solaris专用，进程超过资源限制的时候发送
> 
> 注意事项: int execvp(const char* file, const char* argv[]);
> * (1) 第一个参数是要运行的文件，会在环境变量PATH中查找file，并执行. 
> * (2) 第二个参数，是一个参数列表，如同在shell中调用程序一样，参数列表为0，1，2，3……因此，
> wensen.sh 作为第0个参数，需要重复一遍. 
> * (3)  argv列表最后一个必须是 NULL.
> * (4) 失败会返回－1， 成功无返回值，但是，失败会在当前进程运行，执行成功后，直接结束当前进程，可以在子进程中运行.
> ```
>

