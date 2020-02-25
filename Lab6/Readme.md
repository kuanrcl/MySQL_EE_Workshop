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

###











