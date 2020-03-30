# Using Hortonworks Data Platform (HDP) with MySQL
Big Data using Hadoop technology has become the mainstream data analytics platform for ingesting massive amount of data from 
various sources, one of the major data sources is the transactional data such as MySQL. 

## Install Hortonworks Data Platform (HDP) Sandbox
Download and import HortonWorks Sandbox (for Virtualbox) from https://www.cloudera.com/downloads/hortonworks-sandbox.html
Once you downloaded, import the file into Virtualbox and launch the VM once imported into Virtualbox

Log into the URL displayed on the VM splash screen, http://localhost:1080 using user and password **(maria_dev/maria_dev)**
![home](img/H2.png)

Click on **Launch Dashboard**, you will be taken to HDP homepage
![home2](img/H1.png)

## Download sample dataset
We will use a sample movie dataset for our lab. 
```
wget http://media.sundog-soft.com/hadoop/movielens.sql
wget http://media-sundog-soft.com/hadoop/ml-100k/u.data
wget http://media-sundog-soft.com/hadoop/ml-100k/u.item
```

## Create database in MySQL
```
mysql -uroot -phortonworks1
```
MySQL>
```
create database movielens;
set names 'utf8';
set character set utf8;
source movielens.sql;
create user ''@'localhost' identified by '';
grant all privileges on *.* to ''@'localhost';
```

## Import data from MySQL to Hadoop
Use sqoop to import from MySQL into Hadoop file system and import it to Hive. Sqoop is using MapReduce to sort the data and generate data file in HDFS
```
sqoop import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver --table movies --table movies -m 1 --hive-import
```
### Check the data file uploaded to HDFS
Log on to Hortonworks portal to check the imported data file
Select "File view", navigate to "/home/maria_dev/movies", open the file "part-m-00000"
![portal](img/H4.png)

### Explore the imported Hive table
Log on to HDP dashboard using user/password (maria_dev/maria_dev)
Select the "Hive View 2.0" on the top right corner of the dashboard

![hive](img/H5.png)

Viola!

## Using Python to work with HBase

Start HBase services using HDP Dashboard
Login to HDP VM and start the REST server
```
/usr/hdp/current/hbase-master/bin/hbase-daemon.sh start rest -p 8000 --infoport 8001
```
## Using Pig with HBase
HBase architecture consists of HBase Master to manage the HBase data stored in HDFS
![hbase](img/hbase1.png)

Upload u.user to HDFS
```
hbase shell
create 'users', 'userinfo'
list

```
Download the hbase.pig script
```
wget https://media.sundog-soft.com/hadoop/hbase.pig
pig hbase.pig
```
Pig is using MapReduce to upload data
Check the data after the data is loaded into hbase
```
hbase shell
scan 'users'
disable 'users'
drop 'users'
```

### Cassadra
Cassadra is a NoSQL database with CQL language to query the data. Cassadra shard data into multiple nodes that manage a subset of the
sharded data
![cass](img/cass1.png)


### Miscellaneous
If some of the hadoop services are not started, for example, the Name Node service (some errors saying safe mode)

login to VM as root
password: hadoop
you will be prompted to change the password, change the password to **hortonworks1** same as mysql
```
su hdfs
hdfs dfsadmin -safemode leave
Safe mode is OFF
```
Restart the services in the following order: HDFS, YARN, MapReduce





