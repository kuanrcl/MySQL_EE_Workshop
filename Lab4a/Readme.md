# Using mysqlbackup to redirect binlog to other directory
mysqlbackup is a very rich backup utility that can be used to help DBA to perform various tasks
One of the use is to use mysqlbackup relocate binlog files to other directory

## Backup
First of all, check the binary logs
mysql>
```
show binary logs
```
Next, backup the MySQL server
```
mysqlbackup --defaults-file=config/my1.cnf --user=root --host=127.0.0.1 --port=3306 --password --with-timestamp --backup-dir=/home/mysql/backup/full backup-and-apply-log
```
## Restore the data and relocate the binlog and relay log
Make sure that the server is offline, find out the location of the backup created earlier, look at the latest timestamp directory
Specify the new directory where you want to relocate the binlog and relaylog files. Also make sure that the datadir is empty
```
 mysqlbackup --defaults-file=config/my1.cnf --user=root --host=127.0.0.1 --port=3306 --password --backup-dir=/home/mysql/backup/full/2020-03-27_21-08-37 --log-bin=/tmp/3306/binlogdir/binlog --relay-log=/tmp/3306/binlogdir/relaylog --relay-log-index=/tmp/3306/binlogdir/relaylog.index copy-back
```
Bring up the server, you should see the backup binlog files restored to /tmp/3306/binlogdir
```
mysqld_safe --defaults-file=config/my1.cnf &
ls /tmp/3306/binlogdir
```

Voila!
