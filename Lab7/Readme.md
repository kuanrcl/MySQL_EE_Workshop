# MySQL InnoDB Cluster
In this lab, you will learn how to create MySQL InnoDB cluster from scratch using MySQL Shell. 
We will create 3 MySQL server:
MySQLnode1 VM (192.168.56.41): 2 MySQL servers
MySQLnode2 VM (192.168.56.42): 1 MySQL server
## Build InnoDB Cluster
1. Initialize and Start MySQL Servers

### Login to MySQLnode1 (192.168.56.41)
```
cd /opt/download/lab/13-innoDBCluster/01-firstTest
mysqld --defaults-file=config/my1.cnf --initialize-insecure
mysqld --defaults-file=config/my2.cnf --initialize-insecure
mysqld_safe --defaults-file=config/my1.cnf &
mysqld_safe --defaults-file=config/my2.cnf &
```

### Login to MySQLnode2 (192.168.56.41)
```
cd /opt/download/lab/13-innoDBCluster/01-firstTest
mysqld --defaults-file=config/my3.cnf --initialize-insecure
mysqld_safe --defaults-file=config/my3.cnf &
```

## Create the InnoDB Cluster

