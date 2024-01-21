---
title: linux基本命令
date: 2023-10-20 23:11:05
tags: 
---

## 1. 目录操作

### 1.1  切换目录（cd）

```
cd /                 //切换到根目录
cd /bin              //切换到根目录下的bin目录
cd ../               //切换到上一级目录 或者使用命令：cd ..
cd ~                 //切换到home目录
cd -                 //切换到上次访问的目录
cd xx(文件夹名)       //切换到本目录下的名为xx的文件目录，如果目录不存在报错
cd /xxx/xx/x         //可以输入完整的路径，直接切换到目标目录，输入过程中可以使用tab键快速补全
```

### 1.2 查看目录（ls）

```
ls            //查看当前目录下的所有目录和文件
ls -a         //查看当前目录下的所有目录和文件（包括隐藏的文件）
ls -l        //列表查看当前目录下的所有目录和文件（列表查看，显示更多信息），与命令"ll"效果一样
ls /bin      //查看指定目录下的所有目录和文件 

```

### 1.3 创建目录（mkdir）

```
mkdir tools          //在当前目录下创建一个名为tools的目录
mkdir /bin/tools     //在指定目录下创建一个名为tools的目录
```

### 1.4 删除目录与文件（rm）

```
rm 文件名              //删除当前目录下的文件
rm -f 文件名           //删除当前目录的的文件（不询问）
rm -r 文件夹名         //递归删除当前目录下此名的目录
rm -rf 文件夹名        //递归删除当前目录下此名的目录（不询问）
rm -rf *              //将当前目录下的所有目录和文件全部删除
rm -rf /*             //将根目录下的所有文件全部删除【慎用！相当于格式化系统】
```

### 1.5 修改目录（mv）

```
mv 当前目录名 新目录名        //修改目录名，同样适用与文件操作
mv /usr/tmp/tool /opt       //将/usr/tmp目录下的tool目录剪切到 /opt目录下面
mv -r /usr/tmp/tool /opt    //递归剪切目录中所有文件和文件夹
```

### 1.6 拷贝目录（cp）

```
cp /usr/tmp/tool /opt       //将/usr/tmp目录下的tool目录复制到 /opt目录下面
cp -r /usr/tmp/tool /opt    //递归剪复制目录中所有文件和文件夹
```

### 1.7 搜索目录（find）

```
find /bin -name 'a*'        //查找/bin目录下的所有以a开头的文件或者目录
```

### 1.8 查看当前目录（pwd）

```
pwd                         //显示当前位置路径
```

## 2. 文件操作

### 2.1 新增文件（touch）

```
touch  a.txt     //在当前目录下创建名为a的txt文件（文件不存在），如果文件存在，将文件时间属性修改为当前系统时间
```

### 2.2 删除文件（rm）

```
rm 文件名              //删除当前目录下的文件
rm -f 文件名           //删除当前目录的的文件（不询问）
```

### 2.3  编辑文件（vi、vim）

```
vi 文件名              //打开需要编辑的文件
--进入后，操作界面有三种模式：命令模式（command mode）、插入模式（Insert mode）和底行模式（last line mode）
命令模式
-刚进入文件就是命令模式，通过方向键控制光标位置，
-使用命令"dd"删除当前整行
-使用命令"/字段"进行查找
-按"i"在光标所在字符前开始插入
-按"a"在光标所在字符后开始插入
-按"o"在光标所在行的下面另起一新行插入
-按"："进入底行模式
插入模式
-此时可以对文件内容进行编辑，左下角会显示 "-- 插入 --""
-按"ESC"进入底行模式
底行模式
-退出编辑：      :q
-强制退出：      :q!
-保存并退出：    :wq
## 操作步骤示例 ##
1.保存文件：按"ESC" -> 输入":" -> 输入"wq",回车     //保存并退出编辑
2.取消操作：按"ESC" -> 输入":" -> 输入"q!",回车     //撤销本次修改并退出编辑
## 补充 ##
vim +10 filename.txt                   //打开文件并跳到第10行
vim -R /etc/passwd                     //以只读模式打开文件
```

### 2.4 查看文件

```
cat a.txt          //查看文件最后一屏内容
less a.txt         //PgUp向上翻页，PgDn向下翻页，"q"退出查看
more a.txt         //显示百分比，回车查看下一行，空格查看下一页，"q"退出查看
tail -100 a.txt    //查看文件的后100行，"Ctrl+C"退出查看
```

## 5. 文件权限

### 5.1 权限说明

```
文件权限简介：'r' 代表可读（4），'w' 代表可写（2），'x' 代表执行权限（1），括号内代表"8421法"
##文件权限信息示例：-rwxrw-r--
-第一位：'-'就代表是文件，'d'代表是文件夹
-第一组三位：拥有者的权限
-第二组三位：拥有者所在的组，组员的权限
-第三组三位：代表的是其他用户的权限
```

### 5.2 文件权限

```
普通授权    chmod +x a.txt    
8421法     chmod 777 a.txt     //1+2+4=7，"7"说明授予所有权限
```

## 6.打包与解压

### 6.1 说明

```
.zip、.rar        //windows系统中压缩文件的扩展名
.tar              //Linux中打包文件的扩展名
.gz               //Linux中压缩文件的扩展名
.tar.gz           //Linux中打包并压缩文件的扩展名
```

### 6.2 打包文件

```
tar -zxvf a.tar                      //解包至当前目录
tar -zxvf a.tar -C /usr------        //指定解压的位置
unzip test.zip             //解压*.zip文件 
unzip -l test.zip          //查看*.zip文件的内容 
```

6.3 解压文件

```
tar -zxvf a.tar                      //解包至当前目录
tar -zxvf a.tar -C /usr------        //指定解压的位置
unzip test.zip             //解压*.zip文件 
unzip -l test.zip          //查看*.zip文件的内容 
```

## 7. 其他命令

### 7.1 which

```
说明：which指令会在环境变量$PATH设置的目录里查找符合条件的文件。
which bash             //查看指令"bash"的绝对路径
```

### 7.2 grep

```
grep -i "the" demo_file              //在文件中查找字符串(不区分大小写)
grep -A 3 -i "example" demo_text     //输出成功匹配的行，以及该行之后的三行
grep -r "ramesh" *                   //在一个文件夹中递归查询包含指定字符串的文件
```

### 7.3 uname

```
说明：uname可以显示一些重要的系统信息，例如内核名称、主机名、内核版本号、处理器类型之类的信息 
uname -a
```

### 7.4 yum

```
说明：安装插件命令
yum install httpd      //使用yum安装apache 
yum update httpd       //更新apache 
yum remove httpd       //卸载/删除apache 
```

### 7.5 wget

```
说明：使用wget从网上下载软件、音乐、视频 
示例：wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-3.2.1.tar.gz
//下载文件并以指定的文件名保存文件
wget -O nagios.tar.gz http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-3.2.1.tar.gz
```

### 7.6 ftp

```
ftp IP/hostname    //访问ftp服务器
mls *.html -       //显示远程主机上文件列表
```

### 7.7 scp

```
scp /opt/data.txt  192.168.1.101:/opt/    //将本地opt目录下的data文件发送到192.168.1.101服务器的opt目录下
```

## 8. 系统管理和网络

### 8.1 防火墙操作

```
service iptables status      //查看iptables服务的状态
service iptables start       //开启iptables服务
service iptables stop        //停止iptables服务
service iptables restart     //重启iptables服务
chkconfig iptables off       //关闭iptables服务的开机自启动
chkconfig iptables on        //开启iptables服务的开机自启动
##centos7 防火墙操作
systemctl status firewalld.service     //查看防火墙状态
systemctl stop firewalld.service       //关闭运行的防火墙
systemctl disable firewalld.service    //永久禁止防火墙服务
```

### 8.2 查看网络

```
ifconfig
```

### 8.3 修改IP

```
修改网络配置文件，文件地址：/etc/sysconfig/network-scripts/ifcfg-eth0
------------------------------------------------
主要修改以下配置：  
TYPE=Ethernet               //网络类型
BOOTPROTO=static            //静态IP
DEVICE=ens00                //网卡名
IPADDR=192.168.1.100        //设置的IP
NETMASK=255.255.255.0       //子网掩码
GATEWAY=192.168.1.1         //网关
DNS1=192.168.1.1            //DNS
DNS2=8.8.8.8                //备用DNS
ONBOOT=yes                  //系统启动时启动此设置
-------------------------------------------------
修改保存以后使用命令重启网卡：service network restart
```

### 8.4 配置映射

```
修改文件： vi /etc/hosts
在文件最后添加映射地址，示例如下：
192.168.1.101  node1
192.168.1.102  node2
192.168.1.103  node3
配置好以后保存退出，输入命令：ping node1 ，可见实际 ping 的是 192.168.1.101。
```

### 8.5 查看进程

```
ps -ef         //查看所有正在运行的进程
```

### 8.6 结束进程

```
kill pid       //杀死该pid的进程
kill -9 pid    //强制杀死该进程   
```

### 8.7 启动应用服务

```
systemctl start nginx.service		//启动nginx
systemctl start httpd.service		//启动阿帕奇
```

### 8.8 查看服务及监听端口（netstat）

```
netstat -ntlp   				//查看当前所有tcp端口·
netstat -ntulp |grep 80   		//查看所有80端口使用情况·
netstat -ntulp |grep nginx   	//查看所有nginx服务端口使用情况·
netstat -an | grep 3306   		//查看所有3306端口使用情况·
netstat -nlp |grep LISTEN   	//查看当前所有监听端口·
```

