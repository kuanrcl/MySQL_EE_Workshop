# MySQL Adminstration
In this lab, you will start learning about how to start/stop MySQL server and execute basic commands
## Initialize MySQL Server
1. Go to the lab directory
```
cd /opt/download/lab/02-Admin
export PATH=/usr/local/mysql/bin:$PATH
view /opt/download/lab/02-Admin/config/my.cnf
```
2. Take note on the location of the error log file, data directory, etc

3. Initialize the MySQL server
```
mysqld --defaults-file=config/my.cnf --initialize
```

4. Start up MySQL Server
```
mysqld –defaults-file=config/my.cnf &
```

5. Copy the temporary password displayed on the screen, and find out about the temporary password generated in the error log file

6. Change the root password
```
mysql -uroot –p -h127.0.0.1 -P3306
set password='mysql';
restart;
shutdown;
```

7. Restart the MySQL server
```
mysqld_safe --defaults-file=config/my.cnf &
mysql -uroot –p -h127.0.0.1 -P3306
restart;

## Explore MySQL server
1. Take a look at the MySQL server settings and configurations
```
mysql -uroot –p -h127.0.0.1 -P3306
show variables like ‘general_log%’;
set global general_log=true;
```



