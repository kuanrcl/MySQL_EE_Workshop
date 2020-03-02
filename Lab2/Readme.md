# MySQL Adminstration
In this lab, you will start learning about how to start/stop MySQL server and execute basic commands
## Initialize MySQL Server
1. Go to the lab directory
```
cd /opt/download/lab/02-Admin
export PATH=/usr/local/mysql/bin:$PATH
view /opt/download/lab/02-Admin/config/my1.cnf
```
2. Take note on the location of the error log file, data directory, etc

3. Initialize the MySQL server
```
mysqld --defaults-file=config/my1.cnf --initialize
```

4. Start up MySQL Server
```
mysqld –-defaults-file=config/my1.cnf &
```

5. Copy the temporary password displayed on the screen, and find out about the temporary password generated in the error log file

6. Change the root password
```
mysql -uroot –p -h127.0.0.1 -P3306
```
mysql>
```
set password='mysql';
restart;
shutdown;
```

7. Restart the MySQL server
```
mysqld_safe --defaults-file=config/my1.cnf &
mysql -uroot –p -h127.0.0.1 -P3306
```
mysql>
```
restart;
```

## Explore MySQL server
1. One 1st terminal, monitor the MySQL error log
```
tail -f /home/mysql/data/02/virtual-41.log
```

2. On 2nd Terminal, take a look at the MySQL server settings and configurations
```
mysql -uroot -h127.0.0.1 -P3306 -p
```
mysql>
```
show variables like ‘general_log%’;
set global general_log=true;
select 1;
restart;
show variables like 'general_log%';
set persist general_log=true;
restart;
\q
```

3. Try more commands
```
mysql -uroot -h127.0.0.1 -P3306 -p
```
mysql>
```
status;
show status;
show status\G;
show global status like 'COM%';
show variables;
show global variables;
\q
```

## Creating Users
1. Create a new user
```
mysql -uroot -h127.0.0.1 -P3306 -p
```
mysql>
```
create user demo1@’%’ identified by ‘demo1’;
\q
```

2. Login using demo1 user, and take note on the full user id
```
mysql -udemo1 -h127.0.0.1 -P3306 -p
```
mysql>
```
select user(), current_user();
\q
```








