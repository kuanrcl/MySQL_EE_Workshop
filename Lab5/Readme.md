# MySQL Enterprise Monitor
In this lab, you will install MySQL Enterprise Monitor on MySQLnode2 (192.168.56.42) and learn how to monitor MySQL instances

## MySQL Enterprise Monitor Installation (MySQLnode2: 192.168.56.42)
1. Install MySQL server, MySQL Router, MySQL Shell
```
cd /opt/download/lab/01-install
cd 01-mysql-server
sudo ./05-installTar.sh
cd ../02-router
sudo ./installTar.sh
cd ../03-shell
sudo ./installTar.sh
```

2. Install MySQL Enterprise Monitor
```
cd /opt/download/mysql/mem8
sudo chmod +x ./*.bin
./mysqlmonitor-8.0.18-1217-linux-x86_64-installer.bin
```
Just hit "Enter"-key for all the question prompts except the password
```
password: mysql
```

3. Once installed, you can use your local browser to navigate to https://192.168.56.42:18443

4. Enter the following in the user fileds:
Create user with 'manager' role
```
Username: admin
Password: admin
```
Create user with 'agent' role
```
Username: agent
Password: agent
```

![Monitor](img/MON1.png)

## Install MySQL Enteprise Monitor Agent (MySQLnode1: 192.168.56.41)
1. Install MySQL Enterprise Monitor Agent
```
cd /opt/download/mysql/mem8/
sudo chmod +x *.bin
./mysqlmonitoragent-8.0.18-linux-x86-64bit-installer-bin
```

Enter the following:
```
IP Address for the Monitor: 192.168.56.42
Port: <Enter>
Agent User: <Enter>
Password: agent
```

![Monitor](img/MON2.png)

2. Start the agent
```
/home/mysql/mysql/enterprise/agent/bin/agent.sh &
```
Or
```
/home/mysql/mysql/enterprise/agent/etc/init.d/mysql-monitor-agent start
```

## What are you looking at in MySQL Enterprise Monitor (MEM)?

There is a lot of information in MEM, it can be overwhelming if you don't know what you are looking for or looking at. Let's expore what is available in MEM and how you can use MEM to help you understand MySQL better

### MEM Main Dashboard

The main dashboard displays the following information
1. A list of Timeseries graphs
2. MySQL instances monitored, problematic instances, hosts, and database activities
3. Critical events

![mem-34](img/mem-34.png)

### Events

You can check the Events tab for critical events for immediate remedy or actions
![mem-39](img/mem-39.jpg)

### Monitor MySQL instances

You will add new instances to MEM dashboard by specifying the hosts/ports of the MySQL instances to be added to MEM dashboard

![mem-6](img/mem-6.jpg)

Once the instance is added to MEM, you can then navigate the various configuration tabs to inspect the current configuration details of the monitored MySQL instance

![mem-7](img/mem-7.png)

### What are you looking for and looking at?

You can find lots of useful metrics by selecting the various metrics on the left panel of the MEM dashboard
* Timeseries Graph

![mem-8](img/mem-8.png)

* Table Statistics

![mem-9](img/mem-9.jpg)

* User Staticstics

![mem-10](img/mem-10.png)


* Memory Usage

![mem-11](img/mem-11.jpg)


* Database File I/O
** By File Type
![mem-12](img/mem-12.jpg)

![mem-13](img/mem-13.png)

** By Wait Type
![mem-14](img/mem-14.jpg)

** By Thread Type
![mem-15](img/mem-15.png)

* InnoDB Buffer Pool

You need to click on the **Generate Report** to see the InnoDB Buffer utilization
![mem-16](img/mem-16.jpg)

![mem-17](img/mem-17.png)

* Processes

![mem-18](img/mem-18.jpg)


* Lock Waits

Row Lock

![mem-37](img/mem-37.jpg)

Table Lock

![mem-36](img/mem-36.jpg)

## Query Analyzer

It is very common that users encountered slow response time when executing queries, you can turn to MEM to monitor slow running queries from time to time to fine tune the database to improve these slow running queries. MEM provides **Query Analyzer** to categorize slow running queries by color using **Query Response Time Index (QRTi)** in the following categories:
1. Less than 100ms (Green)
2. Between 100ms and 400ms (Yellow)
3. More than 400ms (Red)

![mem-23](img/mem-23.jpg)

![mem-24](img/mem-24.png)

You can drill into one of these queries for more details

![mem-25](img/mem-25.png)

![mem-26](img/mem-26.png)

## Replication

Besides standalone MySQL instances, you can also monitor MySQL replica and InnoDB Cluster

![mem-27](img/mem-27.png)

![mem-28](img/mem-28.png)

![mem-29](img/mem-29.jpg)

![mem-35](img/mem-35.jpg)

## Backups

You can also monitor the status of database backup
![mem-30](img/mem-30.png)
![mem-31](img/mem-31.png)
![mem-32](img/mem-32.png)

## Advisors

MEM comes bundled with more than 200 Advisors to monitor critical database events using a set of threadholds, you can create event handlers to define actions whenever any of these threadhold is breached, for example, trigger an email, SMTP traps to alert the DBA
![mem-33](img/mem-33.png)







