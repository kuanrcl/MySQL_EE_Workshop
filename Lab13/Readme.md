# Using Hortonworks Data Platform (HDP) with MySQL
Big Data using Hadoop technology has become the mainstream data analytics platform for ingesting massive amount of data from 
various sources, one of the major data sources is the transactional data such as MySQL. I

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
Use sqoop to import from MySQL into Hadoop file system, sqoop is using MapReduce to sort the data and generate data file in HDFS
```
sqoop import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver --table movies --table movies -m 1
```
### Check the imported data file
Log on to Hortonworks portal to check the imported data file
Select "File view", navigate to "/home/maria_dev/movies", open the file "part-m-00000"
![portal](img/H4.png)





