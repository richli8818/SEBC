
1. Check vm.swappiness on all your nodes
    Set the value to 1 if necessary

[root@ip-172-31-38-53 /]# swapon -s

Filename				Type		Size	Used	Priority

[root@ip-172-31-38-53 /]# cat /proc/sys/vm/swappiness
60

[root@ip-172-31-38-53 /]# sysctl vm.swappiness=1
vm.swappiness = 1

to make the change persistent across reboot, make the following change:
in "/etc/sysctl.conf' file, add the following:
# change the default swappiness of 60 to 1
vm.swappiness = 1

2. Show the mount attributes of all volumes

[root@ip-172-31-38-53 ~]# df
Filesystem     1K-blocks   Used Available Use% Mounted on
/dev/xvde       41284928 675964  38512224   2% /
tmpfs            7685696      0   7685696   0% /dev/shm
[root@ip-172-31-38-53 ~]# 

[root@ip-172-31-38-53 ~]# cat /etc/fstab

LABEL=centos_root		/        ext4      defaults         0 0

devpts     /dev/pts  devpts  gid=5,mode=620   0 0

tmpfs      /dev/shm  tmpfs   defaults         0 0

proc       /proc     proc    defaults         0 0

sysfs      /sys      sysfs   defaults         0 0

[root@ip-172-31-38-53 ~]# 

3. Show the reserve space of any non-root, ext-based volumes

[root@ip-172-31-38-53 ~]# df / | grep dev | cut -f 3,6 -d\  | awk '{print ($1*.05)+$2}'
0
[root@ip-172-31-38-53 ~]# 

4. Disable transparent hugepages


5. list Inerface configuration:

[root@ip-172-31-32-122 ~]# ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP qlen 1000
    link/ether 06:df:46:51:8d:15 brd ff:ff:ff:ff:ff:ff
    
    
6. install nslookup:
***
[root@ip-172-31-32-122 ~]# nslookup
> ^C[root@ip-172-31-32-122 ~]# nslookup ip-172-31-32-122
Server:		172.31.0.2
Address:	172.31.0.2#53

Non-authoritative answer:
Name:	ip-172-31-32-122.us-west-2.compute.internal
Address: 172.31.32.122

[root@ip-172-31-32-122 ~]# yum install bind-utilsD
***
    
7.    Show the nscd service is running

[root@ip-172-31-32-122 ~]# chkconfig nscd  on
[root@ip-172-31-32-122 ~]# chkconfig --list |grep nscd
nscd           	0:off	1:off	2:on	3:on	4:on	5:on	6:off
[root@ip-172-31-32-122 ~]# /etc/init.d/nscd start
Starting nscd:                                             [  OK  ]
[root@ip-172-31-32-122 ~]# ps -ef |grep nscd
nscd      1100     1  0 01:56 ?        00:00:00 /usr/sbin/nscd
root      1114   943  0 02:00 pts/0    00:00:00 grep nscd
[root@ip-172-31-32-122 ~]# 


8. Show the ntpd service is running

[root@ip-172-31-32-122 ~]# chkconfig --list |grep ntpd
ntpd           	0:off	1:off	2:on	3:on	4:on	5:on	6:off
ntpdate        	0:off	1:off	2:off	3:off	4:off	5:off	6:off
[root@ip-172-31-32-122 ~]# ps -ef |grep ntpd
ntp        995     1  0 01:34 ?        00:00:00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
root      1132   943  0 02:01 pts/0    00:00:00 grep ntpd
[root@ip-172-31-32-122 ~]# 

MySQL installation - Plan Two Detail

yum install wget

wget http://repo.mysql.com/mysql-community-release-el5-5.noarch.rpm

### not this command: wget http://repo.mysql.com/mysql-community-release5-5.noarch.rpm

rpm -ivh mysql-community-release-el5-5.noarch.rpm

### use the following commands (vi) to comment out all items under section [mysql57-community-dmr]

vi /etc/yum.repos.d/mysql-community.repo 

yum list all | grep -i mysql

yum install mysql-community-server

yum --skip-broken install mysql-community-server   ### if the above doesn't work or with errors.

## install MYSQL

yum --skip-broken install mysql






