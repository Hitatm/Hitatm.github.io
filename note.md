目录 
> [TOC]

> Linux 基本命令复习
# 快捷键

|按键|  作用|
|---------|-----------|
|    Ctrl+d | 键盘输入结束或退出终端|
 |   Ctrl+s | 暂定当前程序，暂停后按下任意键恢复运行|
|    Ctrl+z | 将当前程序放到后台运行，恢复到前台为命令fg|
|    Ctrl+a  |将光标移至输入行头，相当于Home键|
 |   Ctrl+e | 将光标移至输入行末，相当于End键|
 |   Ctrl+k | 删除从光标所在位置到行末|
 |   Alt+Backspace |  向前删除一个单词|
 |   Shift+PgUp | 将终端显示向上滚动|
 |   Shift+PgDn | 将终端显示向下滚动|

#shell通配符使用


|字符  |含义|
|--------|---------|
|*  | 匹配 0 或多个字符|
|?  | 匹配任意一个字符|
|[list] | 匹配 list 中的任意单一字符|
|[!list] |匹配 除list 中的任意单一字符以外的字符|
|[c1-c2] |匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]|
| \{string1,string2,...\}  |  匹配 sring1 或 string2 (或更多)其一字符串|
|\{c2..c2\}   | 匹配 c1-c2 中全部字符 如{1..10}|

> touch linux_{1..10}_txt.txt 新建多个文件。



#用户管理
* who am i
* groups  username 查看用户所属组
* usermod  可以改变用户所属组
>例如 sudo usermod -G sudo xiaoming 
>将xiaoming用户添加到sudo用户组。
*deluser 删除用户
> 例如： sudo deluser  xiaoming --remove-home 删除xiaoming用户同时删除用户主目录。

* ls -AlsSh   -s(显示size) -h(方便人看的格式显示) -A -S(按照size排序显示) 
* chown 
* chmod  go +-rwx  **，一个目录要同时具有读权限和执行权限才可以打开，而一个目录要有写权限才允许在其中创建其它文件**

#文件搜索命令 
> whereis,which,find,locate
### whereis 
* whereis只能搜索二进制文件(-b)，man帮助文件(-m)和源代码文件(-s)。如果想要获得更全面的搜索结果可以使用locate命令。
### locate
* locate 命令使用前首先要手动执行下 updatedb 这样能够保证mlocate.db数据库是最新更新的。
* 查找 /usr/share/ 下所有 jpg 文件：locate /usr/share/\*.jpg "\"转义反斜杠必须加上。
* locate -c [查找项目] 表示 只返回个数
* locate -i [查找项目] 表示 忽略大小写
* 具体可以man一下
### which 
>which本身是 Shell 内建的一个命令，通常使用which来确定是否安装了某个指定的软件，因为它只从PATH环境变量指定的路径中去搜索命令.
### find 命令很强大
能够根据文件的修改时间进行查找。
与时间相关的命令参数：

|参数 | 说明|
|--------|----------|
|-atime  |最后访问时间|
|-ctime | 创建时间|
|-mtime  |最后修改时间|

下面以-mtime参数举例：

|命令|解释|
|-----------|-----------|
|-mtime n: |n 为数字，表示为在n天之前的”一天之内“修改过的文件|
|-mtime +n:| 列出在n天之前（不包含n天本身）被修改过的文件|
|-mtime -n: |列出在n天之前（包含n天本身）被修改过的文件|
|newer file:| file为一个已存在的文件，列出比file还要新的文件名|
|find ~ -mtime 0 | 列出 home 目录中，当天（24 小时之内）有改动的文件|
|find ~ -newer ~/Code|  列出用户家目录下比Code文件夹新的文件|

# 文件打包与压缩
###zip
-r参数表示递归打包包含子目录的全部内容
-q参数表示为安静模式，即不向屏幕输出信息
-o，表示输出文件，需在其后紧跟打包输出文件名
-e 加密选项。
-[n] n可以是1~9表示的是压缩级别。

###unzip 解压文件
-O GBK 指定编码类型
-l选项表示 不解压 只看里边有什么文件

### rar 
**注意：rar 的命令参数没有-，如果加上会报错**
a 添加
d 删除
l list content of archive
e 直接加压到当前目录
x 指定解压目录

### tar
在 Linux 上面更常用的是tar工具，tar 原本只是一个打包工具，只是同时还是实现了对 7z，gzip，xz，bzip2 等工具的支持，这些压缩工具本身只能实现对文件或目录（单独压缩目录中的文件）的压缩，没有实现对文件的打包压缩。
-j 对 bzip2的支持
-z 对gizp的支持
-X 过滤条件
-x extract files from an archive
-t list the contents  of archive

#文件系统操作与磁盘操作
* -h  
* df  ：display file system 展示文件系统信息
* du  ：Summarize disk usage of each FILE，recursively for directories。
  * du -d 1 表示只查看2级目录的 磁盘使用量。
* dd 命令
  dd if=xx  of=xx bs=1M count=256
* mkfs 格式化磁盘为某种文件系统
* mount 命令 mount \[options\] \[source\] \[directory\]
  mount \[-o [操作选项]\] \[-t 文件系统类型\] \[-w|--rw|--ro\] \[文件系统源\] \[挂载点\]
* umount 
* fdisk 操作磁盘分区表 进行磁盘分区

#命令执行顺序 与 管道
### 执行顺序
* 可以用 ;;; 分割命令在一行中来依次执行多条linux 命令；
* 也可以用&& || 分别表示指令执行的逻辑 &&表示前边执行成功才会执行后边的 

### 管道
* ‘|’

### 其他命令
* cut 命令，打印每一行的某一字段 print selected parts of lines from each file to standard output
    *  例如 cut /etc/passwd -d ':' -f 1,6 每行内容的分隔符是： 返回其中第1个第6个域的内容。
    * cut /etc/passwd -c -5 显示每行中前5个字符。
    * cut /etc/passwd -c 2-5 显示每行中前2-5个字符。

* grep 命令在文本中或者stdin中查找匹配字符串
    * -r -n -I  递归搜索子目录文件、打印匹配行号 、忽略二进制文件。
    * -G 正则表达式  -P 支持perl正则表达式

* wc print newline,word,byte counts for each file and total line if more than one file is specified.
    * -c print byte count 
    * -l  lines 
    * -L print lenght of the longest line
    * -w print word  count
    * 例如 ls -dl  /etc/\*/ | wc -l  ; 列出etc目录下的 文件夹个数，使用了管道和wc命令

* sort sort lines of files
    * -r reverse 
    * -t 分隔符 
    * -k  key 排序关键字 sort via a key 
    * -n 按照数字序来排 默认是字典序

* history 查看过去使用过的命令

* uniq 去重复 只去除 相同且连续的行的重复

> 那么问题来了: 如何查看过去使用的过的命令 不能重复?
>   answer ： history | cut -c 8- | cut -d ' ' -f 1 | sort |uniq 
> 首先列出使用过的命令| 删除每行的行号| 只保留每行的第一个数据域|排序|去重
http://image2.wangchao.net.cn/pic/1368526533709.jpg
# 数据流重定向

## 标准输入、输出、标准错误
|文件描述符| 设备文件 | 说明|
|-------|-------|-----------|
|0|/dev/stdin  |标准输入|
|1 |/dev/stdout |标准输出|
|2 |/dev/stderr |标准错误|

* 数据流输出重定向 标准输出和标准错误是不同的 如果需要把 ** 标准错误重定向到标准输出 ** 使用命令 ** 2>&1**
例如 sdf > sss.t  2>&1 这时候会发现 sss.t文件中 有错误提示 command not found :sdf;
* tee 将标准输入中 copy数据到 输入文件 和标准输出中 
  * 例如 echo “sdfsdf” | tee hello 将 sdfsdf 输出到 hello中 同时在标准输出中显示 “sdfsdf”
  
##永久重定向
* exec命令实现永久重定向 
  *  ** exec 1> file ** 那么以后每个命令的标准输出 都会定向到文件file中。
  *  exit 退出 永久重定向

## 特殊的
> **/dev/null，或称空设备，是一个特殊的设备文件，它通常被用于丢弃不需要的输出流，或作为用于输入流的空文件，这些操作通常由重定向完成。读取它则会立即得到一个EOF。**
> 例如 在命令后追加 “1>/dev/null 2>&1” 会使得你的命令执行什么也得不到



# 好玩的
printerbanner 
banner
toilet
figlet
asiccview 
aafire 
cmatrix


# 小tips
1. vimdiff工具 比较文件的异同
2. 直接添加环境变量 echo "PATH=$PATH:/home/shiyanlou/mybin" >> .bashrc
**>>表示将标准输出以追加的方式重定向到一个文件中，注>是以覆盖的方式重定向到一个文件中，使用的时候一定要注意分辨。在指定文件不存在的情况下都会创建新的文件**
3. echo $? 输出的数字是上一条命令的返回值

<!-- ![美女图片呦](http://image2.wangchao.net.cn/pic/1368526533709.jpg) -->


```c
 ________________________________________
/ There is no distinctly native American \
| criminal class except Congress.        |
|                                        |
\ -- Mark Twain                          /
 ----------------------------------------
  \                                  ,+*^^*+___+++_
   \                           ,*^^^^              )
    \                       _+*                     ^**+_
     \                    +^       _ _++*+_+++_,         )
              _+^^*+_    (     ,+*^ ^          \+_        )
             {       )  (    ,(    ,_+--+--,      ^)      ^\
            { (@)    } f   ,(  ,+-^ __*_*_  ^^\_   ^\       )
           {:;-/    (_+*-+^^^^^+*+*<_ _++_)_    )    )      /
          ( /  (    (        ,___    ^*+_+* )   <    <      \
           U _/     )    *--<  ) ^\-----++__)   )    )       )
            (      )  _(^)^^))  )  )\^^^^^))^*+/    /       /
          (      /  (_))_^)) )  )  ))^^^^^))^^^)__/     +^^
         (     ,/    (^))^))  )  ) ))^^^^^^^))^^)       _)
          *+__+*       (_))^)  ) ) ))^^^^^^))^^^^^)____*^
          \             \_)^)_)) ))^^^^^^^^^^))^^^^)
           (_             ^\__^^^^^^^^^^^^))^^^^^^^)
             ^\___            ^\__^^^^^^))^^^^^^^^)\\
                  ^^^^^\uuu/^^\uuu/^^^^\^\^\^\^\^\^\^\
                     ___) >____) >___   ^\_\_\_\_\_\_\)
                    ^^^//\\_^^//\\_^       ^(\_\_\_\)
```
