[TOC]
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











# 好玩的
printerbanner 
banner
toilet
figlet


# 小tips
1. vimdiff工具 比较文件的异同
2. 直接添加环境变量 echo "PATH=$PATH:/home/shiyanlou/mybin" >> .bashrc
**>>表示将标准输出以追加的方式重定向到一个文件中，注>是以覆盖的方式重定向到一个文件中，使用的时候一定要注意分辨。在指定文件不存在的情况下都会创建新的文件**