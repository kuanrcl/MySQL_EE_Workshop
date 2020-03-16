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
Understand the important parameters required for replication in my.cnf. Note that the server-id must be different for both master and slave servers
```
log-bin=mysqllog.bin
relay-log=relay.bin
gtid-mode=on
enforce-gtid-consistency
master_info_repository=TABLE
relay_log_info_repository=TABLE
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
```
mysql>
```
drop user if exists repl@'localhost';
create user repl@'localhost' identified with mysql_native_password by 'repl';
grant replication slave on *.* to repl@'localhost';
flush privileges;
drop user if exists demo@'%';
create  user demo@'%' identified by 'demo';
grant all on *.* to demo@'%';
flush privileges;
select @@hostname, @@port, host,user from mysql.user;
\q
```
On MySQL (Slave, 3326)
```
mysql -t -uroot -h127.0.0.1 -P3326 << EOL2
```
mysql>
```
drop user if exists repl@'localhost';
create user repl@'localhost' identified with mysql_native_password by 'repl';
grant replication slave on *.* to repl@'localhost';
flush privileges;
drop user if exists demo@'%';
create  user demo@'%' identified by 'demo';
grant all on *.* to demo@'%';
flush privileges;
select @@hostname, @@port, host,user,plugin from mysql.user;
\q
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

change master to
master_host='127.0.0.1',
master_user='repl',
master_password='repl',
master_port=3316,
master_auto_position=1
for channel 'channel1';

start slave for channel 'channel1';

show slave status for channel 'channel1'\G
```

### Check replication status

On MySQL (Master, 3316)

```
. ./comm.sh
mysql -t -uroot -h127.0.0.1 -P3316
select @@hostname, @@port;
show master status;
\q
```

On MySQL (Slave, 3326)

```
mysql -uroot -h127.0.0.1 -P3326
select @@hostname, @@port;
show master status;
show slave status for channel 'channel1'\G
\q
```

### Replication in Action

On MySQL (Master, 3316)

```
. ./comm.sh
mysql -uroot -h127.0.0.1 -P3316  << EOL1
create database if not exists test1;
use test1;
create table if not exists mytable1 (f1 int not null auto_increment primary key, f2 varchar(20));
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
insert into mytable1 (f2) values ('aaaaaaaaaaaaaaa');
select @@port, count(*) from test1.mytable1;
\q
```

On MySQL (Slave, 3326), you should see the data replicated across from Master, 3316

```
. ./comm.sh
mysql -t -uroot -h127.0.0.1 -P3326  -e "select @@port, count(*) from test1.mytable1;"
```

## MySQL Replication using ReplicaSet with mysqlsh
You are strongly encouraged to use the brand new ReplicaSet to manage MySQL Replication

### Initialize MySQL engine

```
cd /opt/download/lab/16-ReplicaSet
. ./comm.sh
./01-init.sh
./02-startdb.sh
```

### Configure the ReplicaSetInstance

```
. ./comm.sh
mysqlsh
```

mysqlsh>
```
dba.configureReplicaSetInstance('root:@localhost:3310',{clusterAdmin:'rsadmin',clusterAdminPassword:'rspass'});
dba.configureReplicaSetInstance('root:@localhost:3320',{clusterAdmin:'rsadmin',clusterAdminPassword:'rspass'});
dba.configureReplicaSetInstance('root:@localhost:3330',{clusterAdmin:'rsadmin',clusterAdminPassword:'rspass'});
```

MySQL instances configured and users are created
```
Configuring local MySQL instance listening at port 3310 for use in an InnoDB ReplicaSet...

This instance reports its own address as primary:3310
Assuming full account name 'rsadmin'@'%' for rsadmin

The instance 'primary:3310' is valid to be used in an InnoDB ReplicaSet.
Cluster admin user 'rsadmin'@'%' created.
The instance 'primary:3310' is already ready to be used in an InnoDB ReplicaSet.
Configuring local MySQL instance listening at port 3320 for use in an InnoDB ReplicaSet...

This instance reports its own address as primary:3320
Assuming full account name 'rsadmin'@'%' for rsadmin

The instance 'primary:3320' is valid to be used in an InnoDB ReplicaSet.
Cluster admin user 'rsadmin'@'%' created.
The instance 'primary:3320' is already ready to be used in an InnoDB ReplicaSet.
Configuring local MySQL instance listening at port 3330 for use in an InnoDB ReplicaSet...

This instance reports its own address as primary:3330
Assuming full account name 'rsadmin'@'%' for rsadmin

The instance 'primary:3330' is valid to be used in an InnoDB ReplicaSet.
Cluster admin user 'rsadmin'@'%' created.
The instance 'primary:3330' is already ready to be used in an InnoDB ReplicaSet.
```

### Create the ReplicaSet called myrs

```
. ./comm.sh
mysqlsh --uri=rsadmin:rspass@primary:3310
```

mysqlsh>
```
var x = dba.createReplicaSet('myrs')
x.status()
\q
```

ReplicaSet created successfully

```
WARNING: Using a password on the command line interface can be insecure.
A new replicaset with instance 'primary:3310' will be created.

* Checking MySQL instance at primary:3310

This instance reports its own address as primary:3310
primary:3310: Instance configuration is suitable.

* Updating metadata...

ReplicaSet object successfully created for primary:3310.
Use rs.addInstance() to add more asynchronously replicated instances to this replicaset and rs.status() to check its status.

WARNING: Using a password on the command line interface can be insecure.
You are connected to a member of replicaset 'myrs'.
{
    "replicaSet": {
        "name": "myrs",
        "primary": "primary:3310",
        "status": "AVAILABLE",
        "statusText": "All instances available.",
        "topology": {
            "primary:3310": {
                "address": "primary:3310",
                "instanceRole": "PRIMARY",
                "mode": "R/W",
                "status": "ONLINE"
            }
        },
        "type": "ASYNC"
    }
}
```

### Add MySQL instances to the newly created ReplicaSet (Primary:3310, Secondary:3320, 3330)

```
. ./comm.sh
mysqlsh --uri=rsadmin:rspass@primary:3310
```

mysqlsh>
```
var x = dba.getReplicaSet()
x.addInstance('rsadmin:rspass@primary:3320', {recoveryMethod:'Incremental'})
x.addInstance('rsadmin:rspass@primary:3330', {recoveryMethod:'Incremental'})
x.status()
\q
```

MySQL instannces added to the ReplicaSet

```
WARNING: Using a password on the command line interface can be insecure.
You are connected to a member of replicaset 'myrs'.
Adding instance to the replicaset...

* Performing validation checks

This instance reports its own address as primary:3320
primary:3320: Instance configuration is suitable.

* Checking async replication topology...

* Checking transaction state of the instance...

NOTE: The target instance 'primary:3320' has not been pre-provisioned (GTID set is empty). The Shell is unable to decide whether replication can completely recover its state.

Incremental state recovery selected through the recoveryMethod option

* Updating topology
** Configuring primary:3320 to replicate from primary:3310
** Waiting for new instance to synchronize with PRIMARY...

The instance 'primary:3320' was added to the replicaset and is replicating from primary:3310.

Adding instance to the replicaset...

* Performing validation checks

This instance reports its own address as primary:3330
primary:3330: Instance configuration is suitable.

* Checking async replication topology...

* Checking transaction state of the instance...

NOTE: The target instance 'primary:3330' has not been pre-provisioned (GTID set is empty). The Shell is unable to decide whether replication can completely recover its state.

Incremental state recovery selected through the recoveryMethod option

* Updating topology
** Configuring primary:3330 to replicate from primary:3310
** Waiting for new instance to synchronize with PRIMARY...

The instance 'primary:3330' was added to the replicaset and is replicating from primary:3310.

WARNING: Using a password on the command line interface can be insecure.
You are connected to a member of replicaset 'myrs'.
{
    "replicaSet": {
        "name": "myrs",
        "primary": "primary:3310",
        "status": "AVAILABLE",
        "statusText": "All instances available.",
        "topology": {
            "primary:3310": {
                "address": "primary:3310",
                "instanceRole": "PRIMARY",
                "mode": "R/W",
                "status": "ONLINE"
            },
            "primary:3320": {
                "address": "primary:3320",
                "instanceRole": "SECONDARY",
                "mode": "R/O",
                "replication": {
                    "applierStatus": "APPLIED_ALL",
                    "applierThreadState": "Slave has read all relay log; waiting for more updates",
                    "receiverStatus": "ON",
                    "receiverThreadState": "Waiting for master to send event",
                    "replicationLag": null
                },
                "status": "ONLINE"
            },
            "primary:3330": {
                "address": "primary:3330",
                "instanceRole": "SECONDARY",
                "mode": "R/O",
                "replication": {
                    "applierStatus": "APPLIED_ALL",
                    "applierThreadState": "Slave has read all relay log; waiting for more updates",
                    "receiverStatus": "ON",
                    "receiverThreadState": "Waiting for master to send event",
                    "replicationLag": null
                },
                "status": "ONLINE"
            }
        },
        "type": "ASYNC"
    }
}
```










