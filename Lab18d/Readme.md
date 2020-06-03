# Upgrade 5.X to 8.0

* In place upgrade
* Custom upgrade

## In-place upgrade (RPM)

Use OCI to demo the RPM upgrade

### 5.6 -> 5.7 -> 8.0
```
sudo yum install mysql-community-5.6*
sudo systemctl stop mysqld
sudo yum upgrade mysql-community-* --disablerepo=mysql* --enablerepo=mysql57-community
sudo systemctl start mysqld
sudo mysql_upgrade
sudo systemctl restart mysqld
mysqlsh -- util checkForServerUpgrade root@localhost --config-path=/etc/my.cnf
sudo yum upgrade mysql-community-* --disablerepo=mysql* --enablerepo=mysql80-community
```

## Custom Install

```
/opt/mysql-advanced-5.6.48-linux-glibc2.12-x86_64  
/opt/mysql-advanced-5.7.30-linux-glibc2.12-x86_64  
/opt/mysql-commercial-8.0.19-el7-x86_64  

sudo ln -s /opt/mysql-advanced-5.6.48-linux-glibc2.12-x86_64 /usr/local/mysql5
cd /opt/download/lab/mysql5
./02.start-db.sh
/usr/local/mysql5
mysqladmin -uroot -h127.0.0.1 -P3306 -p shutdown
cp -R /home/mysql/data/mysql56 /home/mysql/data/mysql57

sudo unlink /usr/local/mysql5
sudo ln -s /opt/mysql-advanced-5.7.30-linux-glibc2.12-x86_64 /usr/local/mysql5
cd /opt/download/lab/mysql5
./02.start-db.sh
cp -R /home/mysql/data/mysql57 /home/mysql/data/mysql8

cd /opt/download/lab/mysql5
./02.start-db.sh
```




