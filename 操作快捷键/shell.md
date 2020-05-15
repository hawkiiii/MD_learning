.bash_file 
source

chmod 777 aa.sh
./aa.sh



ps -A
kill -9 进程号

```shell
systemctl restart network.service
```

```shell
systemctl set-default multi-user.target
systemctl set-default graphical.target 
```

```shell
1 使用free -m查看在未增加swap之前虚拟内存的使用情况


2.使用dd命令创建一个swap文件,大小为1G
dd if=/dev/zero of=/home/swap bs=1024 count=1024000
文件越大，创建的时间越厂


3将文件格式转换为swap格式的
 mkswap /home/swap


4再用swapon命令把这个文件分区挂载swap分区
swapon /home/swap



5用free -m命令进行验证，数字增大了1000



6为防止重启后swap分区变成0，要修改/etc/fstab文件
vi /etc/fstab

7在文件末尾（最后一行）加上
/home/swap swap swap default 0 0

8 100最优先使用swap内存
sudo sysctl vm.swappiness=10

```

```shell
#设置hostname
hostnamectl set-hostname hadoop01
```

```shell
cd /etc/sysconfig/network-scripts/
```

```shell
虚拟机克隆
1.
hostnamectl set-hostname hadoop01
2.
ens33 IP和MAC地址
3.
修改/etc/hosts
```

