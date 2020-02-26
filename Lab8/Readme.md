# MySQL Replication
Starting from 8.0.19, MySQL Replication can be configured using MySQL Shell (mysqlsh), similar to how we will create an InnoDB cluster (as in Lab 7). We will create MySQL Replication using the classic method, and the new ReplicaSet method using mysqlsh

## MySQL Replication Classic
### Initialize the MySQL engine
Create 2 MySQL instances ( Master, port 3316 and Salve, 3326) with datadir (~/data/31a and ~/data/31b)
```
cd /opt/download/lab/11-Replication/1-Replication
. ./comm.sh
./00-createdb.sh
```

### Start MySQL instances
```
01-startdb.sh
```

### Create replication user on both MySQL instances (3316, 3326)
On MySQL (Master, 3316)
```
. ./comm.sh
mysql -t -uroot -h127.0.0.1 -P3316
drop user if exists repl@'localhost';
create user repl@'localhost' identified with mysql_native_password by 'repl';
grant replication slave on *.* to repl@'localhost';
flush privileges;
drop user if exists demo@'%';
create  user demo@'%' identified by 'demo';
grant all on *.* to demo@'%';
flush privileges;
select @@hostname, @@port, host,user from mysql.user;
```
On MySQL (Slave, 3326)
```
mysql -t -uroot -h127.0.0.1 -P3326 << EOL2
drop user if exists repl@'localhost';
create user repl@'localhost' identified with mysql_native_password by 'repl';
grant replication slave on *.* to repl@'localhost';
flush privileges;
drop user if exists demo@'%';
create  user demo@'%' identified by 'demo';
grant all on *.* to demo@'%';
flush privileges;
select @@hostname, @@port, host,user,plugin from mysql.user;
```
### Create Replication 
On MySQL (Master, 3316), initialize the binary log
```
.  ./comm.sh
mysql -urroot -h127.0.0.1 -P3316
reset master;
reset slave
```
On MySQL (Slave, 3326), configure the replication settings
```
mysql -uroot -h127.0.0.1 -P3326 << EOL3326
reset master;
reset slave;
hange master to
master_host='127.0.0.1',
master_user='repl',
master_password='repl',
master_port=3316,
master_auto_position=1
for channel 'channel1';

start slave for channel 'channel1';

show slave status for channel 'channel1'\G
```







