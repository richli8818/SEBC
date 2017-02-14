
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
    
    
    


