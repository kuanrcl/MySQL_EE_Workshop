# MySQL Enterprise Backup
We will use MySQL Enterprise Backup to do backup and restore of MySQL database
## Using MySQL Enterprise Workbench to create backup
1. Start MyQL Enteprirse Workbench, select "Online Backup" on the left panel

![Backup](img/BAC1.png)

2. Create MEB Account by select "Create MEB Account"
```
user id: mysqlbackup
password: mysql
```

![Backup](img/BAC2.png)

Click "OK" to complete the configuration

![Backup](img/BAC3.png)

3. Create a backup schedule by selecting "New Job"

![Backup](img/BAC4.png)

4. once completed all the parameters, a cron job will be created

![Backup](img/BAC5.png)

![Backup](img/BAC6.png)

![Backup](img/BAC7.png)

5. You can execute the schedule job to initiate the backup by clicking "Execute Now"

![Backup](img/BAC8.png)

Backup progress ...

![Backup](img/BAC9.png)


