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

### Monitor MySQL instances

You will add new instances to MEM dashboard by specifying the hosts/ports of the MySQL instances to be added to MEM dashboard

![mem-6](img/mem-6.png)

Once the instance is added to MEM, you can then navigate the various configuration tabs to inspect the current configuration details of the monitored MySQL instance

![mem-7](img/mem-7.png)

### What are you looking for and looking at?

You can find lots of useful metrics by selecting the various metrics on the left panel of the MEM dashboard
* Timeseries Graph
![mem-8](img/mem-8.png)
3. Table Statistics
4. 
5. User Staticstics
6. Memory Usage
7. Database File I/O
8. InnoDB Buffer Pool
9. Processes
10. Lock Waits



