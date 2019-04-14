- [shell编程基础](#shell编程基础)
  - [变量与局部变量](#变量与局部变量)
  - [特殊变量](#特殊变量)
  - [变量，参数展开](#变量，参数展开)
  - [字符串展开](#字符串展开)
  - [变量的删除，取代与替换](#变量的删除，取代与替换)
  - [输入输出-READ](#输入输出-READ)
  - [输入输出-ECHO](#输入输出-ECHO)
  - [输入输出-PRINTF](#输入输出-PRINTF)
  - [函数](#函数)
  - [逻辑判断](#逻辑判断)
  - [流程控制-IF](#流程控制-IF)
  - [流程控制-FOR](#流程控制-FOR)
  - [流程控制-WHILE](#流程控制-WHILE)
  - [流程控制-UNTIL](#流程控制-UNTIL)
  - [流程控制-CASE](#流程控制-CASE)
  - [数组](#数组)
  - [数组操作](#数组操作)
  - [基础正则表达式字符集合](#基础正则表达式字符集合)
  - [sed工具](#sed工具)
  - [shell获取字符串长度](#shell获取字符串长度)

# shell编程基础

## 变量与局部变量

> 变量的定义
>
> ```sh
> a=12
> a=helloword
> a=`pwd`
> a=$a:a
> ```
>

> 局部变量
>
> ```sh
> local a=12
> ```

> 取消变量
>
> ```sh
> unset a
> ```

> 生成环境变量
>
> ```sh
> export
> ```

## 特殊变量

> 位置变量
>
> ```sh
> $0: 		#获取当前执行shell脚本的文件名，还包括路径。
> $n: 		#获取当前执行脚本的第n个参数，n=1...9,如果n大于9，则需要将n使用大括号括起来；
> $*: 		#获取当前shell的所有参数，将所有命令行参数视为单个字符串，相当于“$1$2$3”;
> $#: 		#得到执行当前脚本的参数个数；
> $@: 		#获取最高程序所有参数，并保留参数之间的任何空白，相当于“$1” “$2” “$3”,这是将参数传给其他程序的最好办法
> ```

> 状态变量
>
> ```sh
> $?: 		#判断上一指令是否成功执行，0为成功，非零为不成功
> $$: 		#去当前进程的pid
> $!: 		#上一个指令的pid
> ```

## 变量，参数展开

> ```sh
> ${parameter:-word} 	#如果变量未定义，则表达式的值为word。
> ${parameter:=word} 	#如果变量未定义，则设置变量的值为word，返回表达式的值也是word。
> ${parameter:?word} 	#用于捕捉由于未定义而导致的错误并推出程序。
> ${parameter:+word} 	#如果变量已经定义，返回word，也就是真。
> ${!prefix*}
> ${!prefix@} 				#prefix开头的变量
> ```
>

## 字符串展开

> ```sh
> ${#parameter} 								#输出字符长的长度
> ${parameter:offset} 					#从第offset字符开始截取
> ${parameter:offset:length} 		#从offset字符开始截取，取length长度
> ${parameter#pattern} 					#从头删除最短匹配
> ${parameter##pattern} 				#最长
> ${parameter%pattern} 					#从尾删除最短
> ${parameter%%pattern} 				#从尾删除最长
> ${parameter/pattern/string} 	#第一个匹配被替换
> ${parameter//pattern/string} 	#全部匹配被替换
> ${parameter/#pattern/string} 	#字符串开头的替换
> ${parameter/%pattern/string} 	#字符串结尾的替换
> ${parameter..} ${parameter^^} #全部转换为小写、大写
> ${parameter,} ${parameter^}	 	#首字母转换为小写、大写
> ```
>

## 变量的删除，取代与替换

> | 变量设置方式     | str没有设置          | str为空字符串        | str已设置为非空字符串 |
> | ---------------- | -------------------- | -------------------- | --------------------- |
> | var=${str-expr}  | var=expr             | var=                 | var=$str              |
> | var=${str:-expr} | var=expr             | var=expr             | var=$str              |
> | var=${str+expr}  | var=                 | var=expr             | var=expr              |
> | var=${str:+expr} | var=                 | var=                 | var=expr              |
> | var=${str=expr} |str=expr ; var=expr|str不变  ； var=|str不变 ；var=$str|
> | var=${str:=expr} |str=expr ; var=expr|str=expr ; expr|str不变 ； var=$str|
> | var=${str?expr}  | expr 输出至 stderr   | var=                 | var=$str              |
> | var=${str:?expr} | expr 输出至 stderr   | expr 输出至 stderr   | var=$str              |
>



## 限制命令：ulimit

> ulimit [-SHacdflltu] [配额]
>
> ```bash
> -H		#hard limit,严格的设置，必定不能超过这个设置的数值
> -S		#soft limit,警告的设置，可以超过这个设置值，但是超过则有警告信息
> -a		#后面不接任何选项与参数，可列出所有的限制额度
> -c		#当某些程序发生错误时，系统可能将程序在内存中的信息写成文件
> -f		#此shell 可以建立的最大文件容量，
> -d		#程序可以用的最大段内存
> -l		#可用于锁定(lock)的内存量
> -t		#可使用的最大CPU时间
> -u		#单一使用者可以使用的最大进程数量
> ```

## 输入输出：read

> read  [-options] [variable]
>
> ```sh
> -a array 			#把输入赋值到array中，从索引号零开始
> -d delimiter 	#用字符串deliiter中的第一个字符指示输入结束，而不是一个换行符
> -e  					#使用readline来处理输入。这使得与命令行相同的方式编辑输入
> -n num 				#读取num个输入字符，而不是整行
> -p prompt 		#为输入显示提示信息， 使用字符串prompt
> -r 						#Raw mode 不把反斜杠字符解释为转义字符
> -s 						#Silent mode寂寞模式
> -t seconds 		#超时
> -u fd 				#使用文件描述符fd中的输入，而不是标准输入
> ```
>
> 

## 变量声明：declare

> declare [-aixr] variable
>
> ```bash
> -a		#将后面名为 variable 的变量定义成为数组 (array) 类型
> -i		#将后面名为 variable 的变量定义成为整数 (integer) 类型
> -x 		#用法与 export 一样，就是将后面的 variable 变成环境变量
> -r		#将变量设置成为 readonly 类型，该变量不可被更改内容，也不能 unset
> ```



## 输入输出：echo

> echo string
>
> ```sh
> echo -e "Hello HaiZei\n" #开启转义
> echo "Hello $name, This is HaiZei"
> echo "\"Hello HaiZei\""
> ```

## 输入输出：printf

> ```bash
> printf format-string [arguments...]
> ```

## 命令别名：alias

> ```bash
> alias rm='rm -i'
> ```

## 历史命令：history

> history [-option] histfiles
>
> ```bash
> n		#数字，意思是要列出最近的n条命令表的意思
> -c		#将目前的 shell 中的所有 history 内容全部清除
> -a		#将目前新增加的 history 命令新增入 histfiles 中，若没有加 histfiles，则默认写入～/bash history
> -r		#将 histfiles 的内容读到目前这个 shell 的 history 记录中
> -w		#将目前的 history 记录的内容写入 histfiles 中
> ```



## 函数定义

> ```sh
> #!/bin/bash
> function _printf_ {
>     echo $1
>     return 
> }
> 
> _printf_() {
>     echo $1
>     return
> }
> 
> ```
>

## 逻辑判断

> ```sh
> 
> -b #如果文件存在并且是块特殊文件，则为True。
> -c #如果文件存在且是字符特殊文件，则为True。
> -d #如果文件存在并且是一个目录，则为。
> -e #如果文件存在(无论类型)，则文件为True。
> -f #如果文件存在并且是常规文件，则为-f文件True。
> -g #如果文件存在并设置了它的set group ID标志，则该文件为True。
> -h #如果文件存在且是符号链接，则-h文件为真。保留此操作符是为了与此程序的以前版本兼容。不依赖它的存在;使用-l。
> -k #文件为真，如果文件存在，并且它的粘性位被设置。
> -n #如果字符串的长度非零，则为-n字符串True。
> -p #如果文件是命名管道(FIFO)，则为-p文件True。
> -r #如果文件存在且可读，则为-r文件True。
> -s #如果文件存在且大小大于零，则为-s文件True。
> -t #file_descriptor如果文件描述符编号为file_descriptor的文件打开并与终端关联，则为。
> -u #如果文件存在并且设置了它的设置用户ID标志，则该文件为True。
> -w #如果文件存在且可写，则w文件为真。True仅指示写标志处于打开状态。该文件甚至不能在只读文件系统上写入如果测试结果为真。
> -x #如果文件存在且可执行，则该文件为True。True仅指示执行标志处于打开状态。如果file是一个目录，则true表示该文件可以搜索。
> -z #如果字符串的长度为0，则-z字符串为真。
> -L #如果文件存在并且是符号链接，则为。
> -O #如果文件存在且其所有者与此进程的有效用户id匹配，则文件为True。
> -G #如果文件存在且其组与此进程的有效组id匹配，则为-G文件True。
> -S #如果文件存在且是套接字，则为-S文件True。
> 
> file1 -nt file2 #如果file1存在并且比file2更新，则为True。
> 
> file1 -ot file2 #如果file1存在且大于file2，则为True。
> 
> file1 -ef file2 #如果file1和file2存在并引用同一文件，则为True。
> 
> string 				#如果字符串不是空字符串，则为True。
> s1 = s2       #如果字符串s1和s2相同，则s1 = s2为真。
> s1 != s2      #如果字符串s1和s2不相同，则s1 != s2为真。
> s1 < s2       #如果字符串s1根据字符的二进制值出现在s2之前，则s1 < s2为真。
> s1 > s2       #如果字符串s1根据字符的二进制值位于s2之后，则s1 > s2为真。
> n1 -eq n2     #如果整数n1和n2在代数上相等，则n1 -eq n2为真。
> n1 -ne n2     #如果整数n1和n2在代数上不相等，n1 -ne n2为真。
> n1 -gt n2     #如果整数n1在代数上大于整数n2，则n1 -gt n2为真。
> n1 -ge n2     #如果整数n1在代数上大于或等于整数n2，则n1 -ge n2为真。
> n1 -lt n2     #如果整数n1在代数上小于整数n2，那么n1 -lt ln2为真。
> n1 -le n2	  	#如果整数n1在代数上小于或等于整数n2，那么n1 -le ln2为真。
> expression1 -a expression2 #如果表达式1和表达式2都为真，则为真。
> expression1 -o expression2 #如果表达式1或表达式2为真，则为真。
> -a 运算符的优先级高于 -o 运算符。
> 任务：
> find . -name "*.c" | xargs cat -n a | wc -l
> ```
>



## 流程控制：if

> ```sh
> #!/bin/bash
> if [[ condition ]]; then
> 	#statements
> fi
> 
> if [[ condition ]]; then
> 	#statements
> 	else
> 	#statements
> fi
> 
> if [[ condition ]]; then
> 	#statements
> elif [[ condition ]]; then
> 	#statements
> elif [[ condition ]]; then
> 	#statements
> 	else
> 	#statements
> fi
> ```
>

## 流程控制：for

> ```sh
> for i in `seq 1 100`; do
> 	#statements
> 	break
> 	continue
> done
> 
> for (( i = 0; i < 10; i++ )); do
> 	#statements
> done
> ```
>



## 流程控制：while

> ```sh
> #!/bin/bash
> while [[ condition ]]; do
> 	#statementd
> done
> ```
>

## 流程控制：until

> ```sh
> #!/bin/bash
> until [[ conition ]]; do
> 	#statements
> done
> ```
>

## 流程控制：case

> ```sh
> #!/bin/bash
> case word in
> 	pattern1 )
> 	程序段
> 		;;
> 	pattern2 )
> 	程序段
> 		;;
> 	*)
> 		exit 1
> 		;;
> esac
> ```
>

## 数组声明

declare  -a a

> ```sh
> name[subscript]=value
> name=[value1 value2 ...]
> ```

## 数组操作

> 输出数组内容
>
> ```sh
> ${arry[*]}
> ${arry[@]}
> ```
>
> 确定数组元素个数
>
> ```sh
> ${#arry[@]}
> ```
>
> 找到数组的下标
>
> ```sh
> ${!arry[@]}
> ```
>
> 数组追加
>
> ```sh
> arry+=(a b c)
> ```
>
> 数组排序
>
> ```sh
> sort
> ```
>
> 删除数组与元素
>
> ```sh
> unset
> ```
>
> 

## 基础正则表达式字符集合

> | RE字符  | 意义                                         |
> | ------- | -------------------------------------------- |
> | ^word   | 待查找的字符串（word）在行首                 |
> | word$   | 待查找的字符串（word）在行尾                 |
> | .       | 代表【一定有一个任意字符】的字符             |
> | \       | 转义符，将特殊符号的特殊意义去除             |
> | *       | 重复零个到无穷多个的前一个RE字符             |
> | [list]  | 字符集合的RE字符，里面列出想要选取的字符     |
> | [n1-n2] | 字符集合的RE字符，里面列出想要选取的字符范围 |
> | [^list] | 字符集合的RE字符，里面列出不要的字符串或范围 |
> | \{n,m\} | 连续n到m个的【前一个RE字符】                 |
>

## 命名工具：sed

> ```sh
> sed [-nefr]
> -n : 使用安静（slient）模式，在一般sed的用法中，所有来自stdin的数据一般都会被列出到屏幕上。但如果加上-n参数后，则只有经过sed特殊处理的哪一行（或操作）才会被列出来。
> -e : 直接在命令行模式上进行sed的操作编辑
> -f : 直接将sed操作写在一个文本内，-f filename则可以执行filename内的操作
> -r : sed的操作使用的是扩展型正则表达式的语法
> -i : 直接修改读取的文件内容
> a  : 新增，a的后面可以接字符，而这些字符会在新的一行出现（目前的下一行）
> c  : 替换，c的后面可以接字符，这些字符可以替换n1， n2之间的行
> d  : 删除， 因为是删除，所以d后面通常不接任何东西
> i  : 插入， i的后面可以接字符，而这些字符会出现在新一行出现（目前的上一行）
> p  : 打印， 亦即将某个选择的数据打印出来，通常p会与参数sed -n 一起运行
> s  : 替换， 可以直接进行替换的工作，通常这个s的操作可以搭配正则表达式，
> ```
>

## shell获取字符串长度

> 有时在Linux操作系统中需要计算某个字符串的长度，通过查询资料整理了下目前Shell中获取字符串的长度的多种方法，在这里分享给大家，方法如下：
> 方法1: 使用wc -L命令
> wc -L可以获取到当前行的长度，因此对于单独行的字符串可以用这个简单的方法获取，另外wc -l则是获取当前字符串内容的行数。
>
> ```sh
> echo "abc" |wc -L
> ```
>
> 方法2: expr length string
> 使用expr length可以获取string的长度
> 方法3: awk获取域的个数，但是如果大于10个字符的长度时是否存在问题需要后面确认
> 复制代码 代码如下:
>
> ```sh
> echo "abc" |awk -F "" '{print NF}'
> ```
>
> 方法4: 通过awk+length的方式获取字符串长度
> 复制代码 代码如下:
>
> ```sh
> echo “Alex”|awk '{print length($0)}'
> ```
>
> 方法5: 通过echo ${#string}的方式（注意：这里的string是该字符串的变量名）
> 复制代码 代码如下:
>
> ```sh
> name=Alex
> echo ${#name}
> ```

## 测试命令：test

> 1.关于某个文件名的【文件类型】判断，如test -e filename 表示存在否
>
> ```bash
> -e		#该文件名是否存在 （常用）
> -f		#该文件名是否存在且为文件（file）（常用）
> -d		#该文件名是否存在且为目录（directory）（常用）
> -b		#该文件名是否存在且为一个 block device 设备
> -c		#该文件名是否存在且为一个 character device 设备
> -S		#该文件名是否存在且为一个 socket 文件
> -p		#该文件名是否存在且为一个FIFO （pipe）文件
> -L		#该文件名是否存在且为一个链接文件
> ```
>
> 2.关于文件的权限检测，如 test -r filename 表示可读否 （但root权限常有例外）
>
> ```bash
> -r		#检测文件名是否存在且具有【可读】的权限
> -w		#检测文件名是否存在且具有【可写】的权限
> -x		#检测文件名是否存在且具有【可执行】的权限
> -u		#检测文件名是否存在且具有【SUID】的属性
> -g		#检测文件名是否存在且具有【SGID】的属性
> -k		#检测文件名是否存在且具有【Sticky bt】的属性
> -s		#检测文件名是否存在且为【非空文件】
> ```
>
> 3.两个文件之间的比较，如：test file1 -nt file2
>
> ```bash
> -nt		#(newer than) 判断 file1 是否比 file2 新
> -ot		#(older than) 判断 file1 是否比 file2 旧
> -ef		#判断 file1 与 file2 是否为同一个文件，可用在判断 hard link 的判定上。主要意义在判定，两个文件是否均指向同一个文件
> ```
>
> 4.关于两个整数之间的判定，例如 test n1 - n2
>
> ```bash
> -eq		#两数值相等（equal）
> -ne		#两数值不相等（not equal）
> -gt		#n1 大于 n2 （greater than）
> -lt		#n1 小于 n2 （less than）
> -ge		#n1 大于等于 n2 （greater than or equal）
> -le		#n1 小于等于 n2 （less than or equal）
> ```
>
> 5.判定字符串的数据
>
> ```bash
> test -z string		#判定字符串是否为 0？若 string 为空字符串，则为true
> test -n string		#判定字符串是否非为 0？若 string 为空字符串，则为true
> test str1 == str2	#判定 str1 是否等于 str2，若相等，则为true
> test str1 != str2	#判定 str1 是否不等于 str2， 若相等，则返回false
> ```
>
>  6.多重条件判断，
>
> ```bash
> -a		#(and) 两条件同时成立，
> -o		#(or) 两条件任何一个成立。
> !		#反向状态。
> ```

## 判断符号：[ ]

> - 在中括号内的每个组件都需要空格来分隔
> - 在中括号内的变量，最好都以双引号括起来
> - 子啊中括号中常数，最好都以单或双引号括起来

