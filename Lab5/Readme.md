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


