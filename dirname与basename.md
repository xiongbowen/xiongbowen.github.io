---
title: dirname与basename
categories: linux
tags: sh
---
# dirname与basename

dirname 和 basename通常在 shell 内部命令替换使用，以指定一个与指定输入文件名略有差异的输出文件名。

Centos已经默认安装了`basename`命令了，该命令包含在`coreutils`安装包里。

```sh
rpm -qf /usr/bin/basename
```



## dirname - 从文件名剥离非目录的后缀

dirname命令去除文件名中的非目录部分，仅显示与目录有关的内容。dirname命令读取指定路径名保留最后一个/及其后面的字符，删除其他部分，并写结果到标准输出。如果最后一个/后无字符，dirname 命令使用倒数第二个/，并忽略其后的所有字符。

### 命令格式

```shell
[root@localhost test]$ dirname string
```

###  参考示例

#### 1.如果最后一个文件是目录的情形

```sh
[root@localhost test]$ dirname /home/root/share/
/home/root
```

#### 2.如果最后一个文件是普通文件情形

```shell
[root@localhost  ~]$ dirname /home/root/data.txt
/home/root
```

#### 3.如果名字中没有包含/ 则输出 .

```sh
[root@localhost ~]$ dirname dir
.
```

#### 4.相对路径情形

```shell
[root@localhost ~]$  dirname dir/a 
dir
```

#### 5.路径是根目录的情形

```sh
[root@localhost test]$ dirname /
/
[root@localhost test]$ dirname //
/
```



---



## basename - 给定的文件名中删除目录和后缀


`basename`有两种语法：

```sh
basename NAME [SUFFIX]
basename OPTION... NAME...
```

basename最后一部分。也可以删除任何结尾的后缀。这是一个简单的命令，最基本的是去掉文件明前面的目录并打印出来：

```text
[root@localhost ~]# basename /etc/yum.repos.d/CentOS-Base.repo 
CentOS-Base.repo
```

basename命令默认删除所有结尾的`/`字符：

```sh
[root@localhost ~]# basename /usr/local/
local
[root@localhost ~]# basename /usr/local
local
```

默认情况下，每条输出行以换行符(\n)结尾。要以NUL结尾，请使用-z（--zero）选项。

```sh
[root@localhost ~]# basename -z /usr/local
local[root@localhost ~]# 
```

**basename接受多个文件**

basename命令可以接受多个名称作为参数。可以使用-a（--multiple）选项，然后使用空格分隔文件列表。例如，要获取/etc/passwd和/etc/shadow的文件名，可以运行：

```text
[root@localhost ~]# basename -a /etc/passwd /etc/shadow
passwd
shadow
```

**删除指定结尾的后缀**

要从文件名中删除任何结尾的后缀，请将后缀作为第二个参数传递：

```sh
[root@localhost ~]# basename /etc/hostname name
host
另一种方法：
[root@localhost ~]# basename -s name /etc/hostname 
host
```

上面例子中，指定name为后缀，可以看到输出结果中只显示`/`后面和`name`前面的内容了。


通常，此功能用于删除文件的扩展名：

```sh
[root@localhost ~]# basename -s .conf  /etc/httpd/conf/httpd.conf
httpd
或者
[root@localhost ~]# basename /etc/httpd/conf/httpd.conf .conf
httpd
```


下面例子，使用-a选项指定多个文件，-s选项指定后缀内容：

```text
[root@localhost ~]# basename -a -s .conf /etc/sysctl.conf /etc/httpd/conf/httpd.conf 
sysctl
httpd
```

*删除末尾后缀的另一种方法是使用-s（--suffix = SUFFIX）选项指定后缀。上面实例中以展现。*

### 使用实例

以下示例显示了如何在bash中使用for循环、mv命令和basename命令，通过将当前目录下面的图片文件，文件扩展名从“ .jpg”替换为“ .jpeg”：

```text
[root@localhost test]# vim convert.sh 

#!/bin/bash
for file in *.jpg
do
  mv "$file" "$(basename $file .jpg).jpeg"
done
```





