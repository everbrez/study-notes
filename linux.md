## ls [dir]
-h
-l
--full-time
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
## ls
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
## $PATH="$PATH":/(file)

## chgrp [-R] group filename

## chown [-R] user:group filename

## chmod [u/g/o/a][+/-/=][rwx][,[u/g/o/a][+/-/=][rwx]] filename
chmod [num] filename





## uname
-a
-r

## lbs_release
-a
