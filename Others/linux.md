## ls [dir]
-h
-l
--full-time
--time=[ctime atime]
## cd
\- 前一个工作目录
~ 目前用户所在主文件夹
~account account用户所在主文件夹
.
..
## pwd [-P]
## mkdir [-mp]
-m 文件权限
-p 递归创建目录
## rmdir [-rp]
-p 递归
-a
-A 不包括.和..
-d 仅列出目录
-h
-l
-r 反向
-R 递归
-t 时间排序
--full-time
-ll
## cp [-adfiprs] source1 [source2...] directory
-a ==> -pdr
-p 连同文件属性一起复制
-r 递归
-s symbol link
-d 若源文件为link file ，复制连接文件属性而非源文件
-f 强制
-i 若目标文件已经存在，在覆盖时先询问操作的进行（常用）
## rm [file/dir]
-i
-r
-f
## mv source [...source] directory
-f
-i
-u
## touch [-acdmt] file
-a 仅修改访问时间
-c 仅修改文件时间，若不存在则不创建新文件
-m 仅修改mtime
-d 后面接想要修改的时间，也可以用 --date=""
-t 格式【YYMMDDhhmm】
时间：
mtime：modification time
ctime：status time
atime：access time

## cat
-n
-E 打印结尾的断行字符$
-v 列出一些看不出的特殊字符
## tac

## nl

## more
空格 向下翻动一页
Enter: 向下滚动一行
/字符串 正向查找
b 向上翻动一页
:f 显示文件名以及目前显示的行数
## less
空格 向下翻动一页
[PageDown] 向下翻动一页
[PageUp] 向上翻动一页
/字符串 正向查询
？字符串 反向查询
n: 重复前一个查询
N: 重复反向查询
q: 退出程序
## head [-n number] file
默认10行
-n -100 只列出100行之前的数据
## tail [-n number] file
默认10行
-f 持续检测后面的文件名
-n +100 只列出100行后面的数据
显示10-20行：head -n 20 filename | tail -n 10
## od [-t type] file
二进制文件
type:
- a 默认字符
- c 使用ASCII字符
- d[size] 十进制
- f[size] 浮点数
- o
- x

## file 
查看文件类型
-mtime [+-]n
-newer file
-name filename
-size [+-]size
-type [f d l s p b c]
-perm +mode
-user name
-group name

## which
在PATH中查找可执行文件
## whereis
-b 二进制文件
-m 说明文件manual下的文件
-s source文件
-u 不在上述范围的文件
## locate
-i 忽略大小写
-r 后面可接正则
## find [PATH] [option] [action]

## $PATH="$PATH":/(file)

## chgrp [-R] group filename

## chown [-R] user:group filename

## chmod [u/g/o/a][+/-/=][rwx][,[u/g/o/a][+/-/=][rwx]] filename
chmod [num] filename
## umask
后三位：表示创建文件时应该去掉的权限
文件默认：666
目录默认：777

## chattr [+-=][Aaisu] file/dir
-A 访问目录时atime不会改变
-a 数据只能增加不能修改或删除
-i 不能删除改名写入
-s 如果文件被删除将会被完全从磁盘删除
-u 如果文件删除就不会从磁盘删除，可以找回




## uname
-a
-r

## lbs_release
-a
