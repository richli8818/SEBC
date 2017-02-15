
Mysql db config:
root password = mysql

*** Mysql replica configuration:

Node 1 = master (i-0b674d70c641518cd: ec2-52-38-140-117.us-west-2.compute.amazonaws.com, private IP: ip-172-31-35-40)

Node 2 = slave (i-01c253be04326094e: ec2-52-38-107-4.us-west-2.compute.amazonaws.com, private IP: ip-172-31-33-150)


*** 4.  Use /usr/bin/mysql_secure_installation to:
a. Set password protection for the server
b. Revoke permissions for anonymous users
c. Permit remote privileged login
d. Remove test databases
e. Refresh privileges in memory
f. Refreshes the mysqld service


*** 5.  On the master MySQL node, grant replication privileges for your replica node:

[root@ip-172-31-35-40 bin]# mysql -u root -p
Enter password: 
mysql> GRANT REPLICATION SLAVE ON *.* TO 'root'@'ip-172-31-35-40' IDENTIFIED BY 'mysql';
Query OK, 0 rows affected (0.00 sec)

mysql> SET GLOBAL binlog_format = 'ROW'
    -> ;
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH TABLES WITH READ LOCK;
Query OK, 0 rows affected (0.00 sec)

*** 6. In a second terminal session, log into the MySQL master and show its status: 
a. mysql> SHOW MASTER STATUS;
b. Make note of the file name and byte offset. The replica needs this info to sync to the master.
c. Logout of the second session; remove the lock on the first with mysql> UNLOCK TABLES;

[root@ip-172-31-35-40 ~]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 18
Server version: 5.6.35-log MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000001 |     2226 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)

mysql> 

*** c. Logout of the second session; remove the lock on the first with mysql> UNLOCK TABLES;

mysql> UNLOCK TABLES;
Query OK, 0 rows affected (0.00 sec)

*** 7. Login to the replica server and configure a connection to the master:
mysql> CHANGE MASTER TO MASTER_HOST='master host', MASTER_USER='replica user', MASTER_PASSWORD='replica password', MASTER_LOG_FILE='master file name', MASTER_LOG_POS=master file offset;

[root@ip-172-31-33-150 ~]# mysql -u root -p
Enter password: 

mysql> CHANGE MASTER TO MASTER_HOST='ip-172-31-35-40', MASTER_USER='root',MASTER_PASSWORD='mysql', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=2226;
Query OK, 0 rows affected, 2 warnings (0.03 sec)

mysql> 

*** Initiate slave operations on the replica
a. mysql> START SLAVE;
b. mysql> SHOW SLAVE STATUS \G
c. If successful, the Slave_IO_State field will read Waiting for master to send event
d. Once successful, capture this output and store it in labs/2_replica_working.md
e. Review your log (/var/log/mysqld.log) for errors. If stuck, consult with a colleague or instructor.














