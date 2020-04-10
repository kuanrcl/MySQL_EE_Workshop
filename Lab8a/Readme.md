# High Availability with Local- and Cross-site replication
We will configure local replication and cross-site replication to mimic real-life deployment of async replication in production data center and cross-site replication from production data center to disaster recovery data center. In this scenario, Prod DC will have A->B and DRC will have C->D, and A->C
## Local Replication at production site (MySQL Server A and B)
To simulate multiple MySQL servers in a single VM, we will need to specify different **server_id**, **datadir** and **port**
```
my1.cnf
[mysqld]
explicit_defaults_for_timestamp
server-id=41
datadir=/home/mysql/data/41a
basedir=/usr/local/mysql5

port=3346
socket=/home/mysql/data/41a/mysqld.sock

log-error=/home/mysql/data/41a/mysqld.error
sql-mode=NO_ENGINE_SUBSTITUTION,ONLY_FULL_GROUP_BY
log-bin=mysqllog.bin
relay-log=relay.bin

gtid-mode=on
enforce-gtid-consistency
master_info_repository=TABLE
relay_log_info_repository=TABLE
```
```
my2.cnf
[mysqld]
explicit_defaults_for_timestamp
server-id=42
datadir=/home/mysql/data/41b
basedir=/usr/local/mysql5

port=3346
socket=/home/mysql/data/41b/mysqld.sock

log-error=/home/mysql/data/41b/mysqld.error
sql-mode=NO_ENGINE_SUBSTITUTION,ONLY_FULL_GROUP_BY
log-bin=mysqllog.bin
relay-log=relay.bin

gtid-mode=on
enforce-gtid-consistency
master_info_repository=TABLE
relay_log_info_repository=TABLE
```
Next we will create the "repl" user on both servers (A and B):q:q
```
create user repl@'localhost' identified with mysql_native_password by 'repl';
grant replication slave on *.* to repl@'localhost';
```
### Configure replication channel
Since we are setting up 2 fresh servers, we will configure the replication the following way:
```
mysql -uroot -h127.0.0.1 -P3366 -p << EOL3346
reset master;
reset slave;
EOL3346

mysql -uroot -h127.0.0.1 -P3356 -p << EOL3356
reset master;
reset slave;
EOL3356

mysql -uroot -h127.0.0.1 -P3356 -p << EOL1

change master to
master_host='127.0.0.1',
master_user='repl',
master_password='repl',
master_port=3346,
master_auto_position=1,
master_retry_count=5
for channel 'channel2';

start slave for channel 'channel1';

show slave status for channel 'channel1'\G
EOL1
```
## Local Replication at disaster recovery site (C and D)
Similarly, set up the MySQL Server as above
```
my3.cnf
[mysqld]
explicit_defaults_for_timestamp
server-id=43
datadir=/home/mysql/data/41c
basedir=/usr/local/mysql5

port=3366
socket=/home/mysql/data/41c/mysqld.sock

log-error=/home/mysql/data/41c/mysqld.error
sql-mode=NO_ENGINE_SUBSTITUTION,ONLY_FULL_GROUP_BY
log-bin=mysqllog.bin
relay-log=relay.bin

gtid-mode=on
enforce-gtid-consistency
master_info_repository=TABLE
relay_log_info_repository=TABLE
log-slave-update
```
Please note that there is an additional setting in my.cnf, **log-slave-update**. This parameter is important because as C will replicate from A, and propagate the transactions to D. This **log-slave-update** will write the transactions received from A to its own binary log and in turns, the downstream slav (D) will be able to replicate the C in the following topology (A->C->D)
```
my4.cnf
[mysqld]
explicit_defaults_for_timestamp
server-id=41
datadir=/home/mysql/data/41a
basedir=/usr/local/mysql5

port=3346
socket=/home/mysql/data/41a/mysqld.sock

log-error=/home/mysql/data/41a/mysqld.error
sql-mode=NO_ENGINE_SUBSTITUTION,ONLY_FULL_GROUP_BY
log-bin=mysqllog.bin
relay-log=relay.bin

gtid-mode=on
enforce-gtid-consistency
master_info_repository=TABLE
relay_log_info_repository=TABLE
```
