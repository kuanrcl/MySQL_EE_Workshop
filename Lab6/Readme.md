# MySQL Security (Transparent Data Encryption, Enterprise Firewall, Enterprise Audit)

## Transparent Data Encryption (TDE)
### Install the TDE plugin
We will be reusing the MySQL database in 02-Admin lab but with a few modification
```
cd /opt/download/lab/02-Admin
. ./comm.sh
mysqladmin -uroot -h127.0.0.1 -P3306 -p shutdown
cd /opt/download/lab/04-MEB/05-tde
. ./comm.sh
mysqld_safe --defaults-file=config/my1.cnf &
```
The TDE plugin configuration settings in my1.cnf
```
early-plugin-load=keyring_encrypted_file.so
keyring_encrypted_file_data=/home/mysql/data/3306/mysql-keyring/keyring-encrypted
keyring_encrypted_file_password=password
```
Examine the keyring-encrypted file
```
cd ~/data/3306/mysql-keyring/
ll
file keyring-encrypted
strings keyring-encrypted
hexdump keyring-encrypted
```

### Create encrypted tables
```
cd /opt/download/lab/04-MEB/05-tde
. ./comm.sh
./04-TDEtables.sh
```
Results
```
+------------------------+---------------+-------------+
| plugin_name            | plugin_status | load_option |
+------------------------+---------------+-------------+
| keyring_encrypted_file | ACTIVE        | ON          |
+------------------------+---------------+-------------+
+---------+----+-------------+
| mytable | f1 | f2          |
+---------+----+-------------+
| mytable |  1 | hello world |
| mytable |  2 | hello world |
| mytable |  3 | hello world |
+---------+----+-------------+
+-------------+----+-------------+
| mytable_enc | f1 | f2          |
+-------------+----+-------------+
| mytable_enc |  1 | hello world |
| mytable_enc |  2 | hello world |
| mytable_enc |  3 | hello world |
+-------------+----+-------------+
```
### Examine the table files
```
cd ~/data/3306/mydb2
strings mytable.ibd | more
strings mytable_enc.ibd | more
```

## MySQL Enterprise Firewall
We will use MySQL Workbench to work with Enterprise Firewall

### Install the Enterprise Firewall Plugin
Start MySQL Workbench using the connection profile created during VM preparation and click on "Install Firewall"
![Workbench](img/F1.png)

Once the firewall plugin is installed, enable the firewall and other features

![Workbench](img/F2.png)

![Workbench](img/F3.png)

![Workbench](img/F4.png)

![Workbench](img/F5.png)

Once everything is installed, click on "Update Statistics"

![Workbench](img/F6.png)

### Create a test demo user
Create a user called "demo", select authentication "caching_sha2_password", enter password that you like, click "Create"

![Workbench](img/F7.png)

Select the "Administrative Roles" tab, click on "DBA" role for simplicity reason

![Workbench](img/F8.png)

### Set the Firewall in "Recording" mode

![Workbench](img/F9.png)

### In another terminal, execute a few SQL statements using user "demo"

```
mysql -udemo -h127.0.0.1 -P3306 -p
```
mysql>
```
show databases;
use mydb2;
show tables;
select * from mytable;
select * from mytable_enc;
select f1, f2 from mytable_enc where f1=1;
```

Switch to Workbench
Change the "Recording" mode to "Protecting" mode
Select the statement "select * from mytable" and click on "Delete"
Click on "Protecting" mode
Click on "Apply"


Switch to Terminal again
mysql>
``` 
select * from mytable;
```
Firewall will block this SQL statement

![Workbench](img/F10.png)

### MySQL Enterprise Audit
We will first create a login-path to obfuscate the my.cnf configuration to secure the my.cnf configruation settings
#### Initialize the MySQL engine
```
mysqladmin -uroot -h127.0.0.1 -P3306 -p shutdown
cd /opt/download/lab/23-Security/secure-audit-filter/
. ./comm.sh
./00-createdb-secure.sh




















