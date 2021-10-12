# Linux 笔记

## 基本操作

- 关机/重启

```shell
shutdown -h now # 立即关机
shutdown -h 1 # 一分钟后关机
shutdown -r now # 立即重启
halt # 关机
reboot # 重启
sync # 把内存的数据同步到磁盘
```

- 用户管理

```shell
# 添加用户
useradd hope # 添加一个名叫 hope 的用户
useradd -d /home/user hope # 添加一个名叫 hope 的用户 并指定用户的家目录(/home/user)
useradd -g wudang zwj # 添加用户到其他组

# 修改密码
passwd hope  # 修改 hope 用户密码

# 删除用户
userdel hope # 删除一个名叫 hope的用户 保留家目录
userdel -r hope # 删除用户并且删除家目录

# 查询用户
id root # 查询用户信息

# 切换用户
su root # 切换到root用户

# 查看当前用户/登录用户
whoami
```

- 用户组

```shell
# 增加组
groupadd wudang # 创建一个名叫武当的组

# 删除组
groupdel wudang # 删除组

# 修改组
usermod -g shaolin zjw # 更改用户的所在组 
```

- 文件目录

```shell
# 显示当前目录的绝对路径
pwd

# 显示目录和文件
ls 
# 选项
ls -l
ls -a

# 切换目录
cd 

# 创建目录
mkdir 
mkdir -p #创建多个目录

# 删除目录
rmdir # 只能删除空目录 如果是非空目录使用 rm -rf 文件名/文件夹

# 创建空文件
touch

# 拷贝
cp 
cp -r # 递归

# 删除文件/目录
rm 

# 移动文件/重命名
mv

# 查看文件内容(以只读的形式)
cat 
cat -n # 显示行号
cat -n hope.txt | more # 分页的形式展示
more 
less 
heed -n 5 # 显示文件前几行
tail -n 5 # 显示文件后几行
tail -f # 实时更新文件

# 输出重定向/追加
ls -l > a.txt # 输出重定向
ls -l >> a.txt # 追加

# 输出内容
echo

# ln 指令
ln -s /root linkToRoot # 创建软链接
rm -rf linkToRoot # 删除

# history 查看历史
history 
history 10 # 显示最后十个
history !10 # 执行编号为10的指令
```

- 时间日期

```shell
date #显示当前时间
date +%Y #显示当前年份
date -s # 设置时间
cal # 日历
cal 2021 # 显示2021的日历
```

- 搜索查找

```shell
find /opt -name "*.txt" # 按照名字查询
find /opt -user root # 按照用户查询
find /opt -size +20m # 按照文件大小查询
locate # 快速搜索到文件路径 第一次必须执行updatedb
grep -n "a" /opt/hope.txt  # 查找文件包含a的字符 并显示行号
cat /opt/hope.txt | grep -ni a 查找文件包含a的字符 并显示行号不区分大小写
```

- 压缩/解压

```shell
gzip # 压缩
gunzip # 解压
zip # 压缩
zip -r myOpt.zip  /opt # 递归压缩
unzip # 解压
unzip -d /opt # 解压到opt目录
tar -zcvf myLib.tar.gz  a.txt hope.txt  #压缩多个文件
tar -zxvf myLib.tar.gz -C /opt #解压文件
```

## 组管理和权限管理

```shell
chown tom a.txt # 改变文件的所有者
chgrp tom a.txt # 修改文件所在组
chmod u=rwx,g-rx,o=rx abc 
chmod
r = 4 w = 2 x = 1
```

- 任务调度(crond)

```shell
crontab -e # 编辑定时任务
crontab -r # 删除定时任务
crontab -l # 查看定时任务
# e.g: 每隔一分钟输出文件内容 
*/1 * * * * ls -l /etc >> /tmp/to.txt
```

- 进程管理

## 查看系统进程

```shell
ps -aux
ps -ef | grep java
pstree
```

## 终止进程

```shell
kill 进程号
killall 进程名称
kill -9 强制终止
```

## 服务管理

```shell
service 服务名 | start | stop | restart | status
# CentOS 7 以后使用 systemctl
```

## 动态监控

```shell
top 
```

## 下载

```shell
rpm
yum
```

- 常用指令

```shell
# e.g 统计/home文件夹的个数
ls -l /home | grep "^-" | wc -l
# e.g 统计/home文件夹下目录个数
ls -l /home | grep "^d" | wc -l
```

- 安装第三方软件

## 安装JDK

```shell
# 解压 JDK 安装包
tar -zxvf jdk-8u202-linux-x64.tar.gz
# 配置 /etc/profile 文件
JAVA_HOME=/data/Java/jdk1.8.0_202
PATH=/data/Java/jdk1.8.0_202/bin:$PATH
export JAVA_HOME PATH
# 刷新配置文件
source /etc/profile
```

## shell编程

- 变量

```shell
# 查看系统所有变量
set 
# 声明变量
NAME="张三"
echo $NAME
# 声明静态变量
readonly AGE=100
# 删除变量(静态变量不能unset)
unset NAME
# 输出系统变量
echo $PATH
# 将命令的返回值赋值给变量
RESULT=`date`
RESULT=$(date)
# 环境变量(配置文件/etc/profile)
JAVA_HOME=/data/Java/jdk1.8.0_202 # 声明变量
export JAVA_HOME # 导出变量
source /etc/profile # 刷新配置文件 立即生效
# 多行注释
:<<!
  这是多行注释
!
# 位置参数变量
./test.sh 10 20
echo $0
echo $1
# $n n表示数字 $0 代表命令本身 $1 - $9代表第一到第九个参数 十以上参数${n}
# $* 这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体
# $@ 这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待)
# $# 这个变量代表命令行中所有参数的个数)
# 预定义变量
$$ # 当前进程的进程号(PID)
$! # 后台运行的最后一个进程的进程号(PID)
$? 最后一次执行命令的返回状态 0 正确执行 非 0 自行判断
```

- 运算符

```shell
# 基本语法
$((1+2)) 
$[1+2]
expr 1 + 2
```

- 条件判断

```shell
= # 字符串比较
-lt # 小于
-le # 小于等于
-eq # 等于
-gt # 大于
-ge # 大于等于
-f # 文件存在并且是一个常规的文件
-e # 文件存在
-d # 文件存在并且是一个目录
```

- 流程控制

```shell
# if判断
if [ 1 -eg 1 ] then fi
if [ 1 -eg 1 ] then echo "相等" elif [1 -le 0] then echo "" fi
# e.g 输入的参数 大于等于60 输出及格 小于60输出不及格
echo $1
echo ""

if [ $1 -ge 60 ]
then
	echo "及格"
elif [ $1 -lt 60 ]
	then echo "不及格"
fi

# case

# 输入的参数是1 输出周一 2输出周二 其他输出other
echo $1
echo ""

case $1 in
	"1")
echo "周一"
;;
	"2")
	echo "周二"
;;
    *)
echo "other"
esac

# for
# e.g 打印命令行输入的参数

# 使用 $*
for i in "$*"
do
	echo "the num is $i"
done

# 使用 $@

for i in "$@"
do
	echo "the num is $i"
done 

SUM=0
for((i=1;i<=100;i++))  do
    SUM=$[$SUM+$i]
done

echo "the sum is $SUM"

# while
# e.g 统计1+n的值
SUM=0
I=0
while [ $I -le $1 ]
do
	SUM=$[$SUM+$I]
	I=$[$I+1]
	done
echo "sum = $SUM"

# 控制台输入
read -p # 指定读取值的提示符
read -t # 指定读取值时等待的时间
```

- 函数

```shell
# 系统函数
basename /home/aaa/test.txt # 删除所有的前缀包括最后一个 "/"字符 然后显示出来
dirname /home/aaa/test.txt # 返回完整路径最后/的前面部分
# 自定义函数

# 计算两个数字的和
function getSum(){
SUM=$[$n1+$n2]
echo "两数之和 : $SUM"
}
read -p "请输入第一个数字 :" n1
read -p "请输入第二个数字 :" n2
getSum $n1 $n2

```

- shell综合测试

```shell
# 备份mysql数据库
BACKUP=/data/backup/db
# 当前时间作为文件名
DATETIME=$(date +%Y_%m_%d_%H%M%S)
echo "********************开始备份********************"
echo "********************备份路径 $BACKUP/$DATETIME.tar.gz"
# 主机
HOST=localhost
# 用户名
DB_USER=root
# 密码
DB_PWD=root
# 数据库名
DATABASE=test
# 如果备份的路径存在就使用 否则就创建
[ ! -d "$BACKUP/$DATETIME" ] && mkdir -p "$BACKUP/$DATETIME"
# 执行 MySQL 的备份数据库指令
mysqldump -u${DB_USER} -p${DB_PWD} --host=$HOST $DATABASE | gzip > $BACKUP/$DATETIME/$DATETIME.sql.gz
# 打包备份文件
cd $BACKUP
tar -zcvf $DATETIME.tar.gz $$DATETIME
#删除临时文件
rm -rf $BACKUP/$DATETIME
# 删除十天前的备份文件
find $BACKUP -mtime +10 -name "*.tar.gz" -exec rm -rf {} \;
```