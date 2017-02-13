
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
