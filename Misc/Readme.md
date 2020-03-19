# Miscellaneous Handy Tips
## Using Samba to exchange files on Windows
Enable samba on linux VM so that you can copy and paste files from your host Windows OS to Linux VM easily
### Install Samba on Linux VM
```
sudo yum install samba
vi /etc/samba/smb.conf
[root]
        path = /
        read only = no
        guest ok = yes
        create mask = 0755
        directory mask = 0755
        force user = root
        force group = root
 sudo systemctl restart smb
 ```
 ### Map Linux folder on Windows Explorer
 Run "Windows Explorer", Map Network Drive, Specify the IP and the samba folder directive, for example, **192.168.56.41/root**

## NDB Commands
mcm>
```
get mycluster1;
get hostname mycluster1;
get Hostname:ndbd mycluster1;
get H* mycluster1;
get DataMemory:ndbmtd mycluster1;

get *:mysqld mycluster1;
get -d ndb*:mysqld mycluster1;
get -d *memory*:ndbmtd:1 mycluster1;
get --include-defaults DataMemory:ndbmtd mycluster1;
get replicate_ignore_table:mysqld mycluster1;
get -d wait_timeout:mysqld mycluster1;

list sites;
list clusters mysite;
list cluster mycluster1;
list nextnodeids mycluster1;
list processes mycluster1;
mcm> list processes mycluster1;
+--------+----------+---------+
| NodeId | Name     | Host    |
+--------+----------+---------+
| 50     | ndb_mgmd | primary |
| 1      | ndbmtd   | primary |
| 2      | ndbmtd   | primary |
| 49     | mysqld   | primary |
| 51     | ndbapi   | *       |
| 52     | ndbapi   | *       |
| 53     | ndbapi   | *       |
+--------+----------+---------+
7 rows in set (0.04 sec)


list backups mycluster1;
mcm> list backups mycluster1;
+----------+--------+---------+-----------+-------+---------------------+
| BackupId | NodeId | Host    | Timestamp | Parts | Comment             |
+----------+--------+---------+-----------+-------+---------------------+
| None     | 1      | primary |           |       | No backup directory |
| None     | 2      | primary |           |       | No backup directory |
+----------+--------+---------+-----------+-------+---------------------+
2 rows in set (0.01 sec)

show status -r mycluster1;
mcm> show status -r mycluster1;
+--------+----------+---------+---------+-----------+-------------+
| NodeId | Process  | Host    | Status  | Nodegroup | Package     |
+--------+----------+---------+---------+-----------+-------------+
| 50     | ndb_mgmd | primary | running |           | cluster.pkg |
| 1      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 2      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 49     | mysqld   | primary | running |           | cluster.pkg |
| 51     | ndbapi   | *       | added   |           |             |
| 52     | ndbapi   | *       | added   |           |             |
| 53     | ndbapi   | *       | added   |           |             |
+--------+----------+---------+---------+-----------+-------------+
7 rows in set (0.05 sec)


show status -b mycluster1;

mcm> show status -b mycluster1;
+---------------+----------+-----------------------------+
| Command       | Status   | Progress                    |
+---------------+----------+-----------------------------+
| start cluster | finished | 100% [####################] |
+---------------+----------+-----------------------------+
1 row in set (0.02 sec)

show status --cluster mycluster1;

mcm> show status --cluster mycluster1;
+------------+-------------------+---------+
| Cluster    | Status            | Comment |
+------------+-------------------+---------+
| mycluster1 | fully operational |         |
+------------+-------------------+---------+
1 row in set (0.06 sec)

show status --operation mycluster1;

show status --backup mycluster1;

mcm> show status --backup mycluster1;
+------------------------------------------+
| Command result                           |
+------------------------------------------+
| No backup currently active in mycluster1 |
+------------------------------------------+
1 row in set (0.07 sec)

show status --process mycluster1;

mcm> show status --progress mycluster1;
+---------------+----------+----------+
| Command       | Status   | Progress |
+---------------+----------+----------+
| start cluster | finished | 100%     |
+---------------+----------+----------+
1 row in set (0.03 sec)


mcm> show status --process mycluster1;
+--------+----------+---------+---------+-----------+-------------+
| NodeId | Process  | Host    | Status  | Nodegroup | Package     |
+--------+----------+---------+---------+-----------+-------------+
| 50     | ndb_mgmd | primary | running |           | cluster.pkg |
| 1      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 2      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 49     | mysqld   | primary | running |           | cluster.pkg |
| 51     | ndbapi   | *       | added   |           |             |
| 52     | ndbapi   | *       | added   |           |             |
| 53     | ndbapi   | *       | added   |           |             |
+--------+----------+---------+---------+-----------+-------------+
7 rows in set (0.08 sec)

show status --progress mycluster1;
```





