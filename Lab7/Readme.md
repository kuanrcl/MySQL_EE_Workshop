# MySQL InnoDB Cluster
In this lab, you will learn how to create MySQL InnoDB cluster from scratch using MySQL Shell. 
We will create 3 MySQL server:
MySQLnode1 VM (192.168.56.41): 2 MySQL servers
MySQLnode2 VM (192.168.56.42): 1 MySQL server

## Initialize and Start MySQL Servers

1. Login to MySQLnode1 (192.168.56.41)
```
cd /opt/download/lab/13-innoDBCluster/01-firstTest
mysqld --defaults-file=config/my1.cnf --initialize-insecure
mysqld --defaults-file=config/my2.cnf --initialize-insecure
mysqld_safe --defaults-file=config/my1.cnf &
mysqld_safe --defaults-file=config/my2.cnf &
```

2. Login to MySQLnode2 (192.168.56.41)
```
cd /opt/download/lab/13-innoDBCluster/01-firstTest
mysqld --defaults-file=config/my3.cnf --initialize-insecure
mysqld_safe --defaults-file=config/my3.cnf &
```

## Create the InnoDB Cluster

1. MySQLnode1, 192.168.56.41, hostname: primary

```
mysqlsh
```
mysql>
```
dba.configureInstance('root@localhost:3310', {clusterAdmin:'gradmin', clusterAdminPassword:'grpass'})
dba.configureInstance('root@localhost:3320', {clusterAdmin:'gradmin', clusterAdminPassword:'grpass'})
```

2. MySQLnode2, 192.168.56.42, hostname: secondary
```
mysqlsh
```
mysql>
```
dba.configureInstance('root@localhost:3330', {clusterAdmin:'gradmin', clusterAdminPassword:'grpass'})
```

3. MySQLnode1, 192.168.56.41, hostname: primary
```
mysql
```
mysql>
```
\connect gradmin:grpass@primary:3310
dba.createCluster('mycluster')
dba.getCluster().status()
var xx = dba.getCluster()
xx.addInstance('gradmin:grpass@primary:3320')
xx.addInstance('gradmin:grpass@secondary:3320')
xx.status()
```

4. Create mysqlrouter
```
cd /opt/download/lab/13-InnodbCluster/01-firstTest/
./07-startRouter.sh
```
Test the mysqlrouter
```
while [ 1 ]
do
sleep 1
mysql -ugradmin -pgrpass -h127.0.0.1 -P6446 -e "select @@hostname, @@port;"
done
```

Open another terminal to 192.168.56.41
```
export PATH=/usr/local/mysql/bin:/usr/local/shell/bin:$PATH
mysqlsh
```
mysqlsh>
```
\connect gradmin:grpass@primary:3310
var x = dba.getCluster('mycluster')
x.status()
x.setPrimaryInstance('primary:3320')
```

Check the hostname and port values in the 1st terminal









