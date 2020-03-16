# Using Kafka with MySQL
## Install Kafka
The prerequisite of Kafka is the JVM. 
Use the bundled JVM in MySQL, for example, export JAVA_HOME=/usr/local/mysql/enterprise/agent/java

### Confluent Platform Community 
We will install Confluent Platform Community Edition, Confluent Hub Client, Kafka Connect Datagen
First of all, download **Confluent Platform Community Editon** from https://www.confluent.io/download. 
Select the TAR version in the Deploymnet Type
![Install](img/C1.png)

Once downloaded, untar the file
```
cd /opt
sudo tar zxvf /tmp/confluent-5.4.0-2.12.tar.gz
sudo ln -s /opt/confluent-5.4.0 /usr/local/kafka
```
### Confluent Hub Client
Install confluent hub client from https://docs.confluent.io/current/connect/managing/confluent-hub/client.html#hub-linux. Just download and unzip the client

### Install kafka connect datagent
```
confluent-hub install \
--no-prompt confluentinc/kafka-connect-datagen:latest
```
### Configure Confluent Kafka
Once everything is installed, set up the environment
```
vi ~/.bash_profile
export CONFLUENT_HOME=/usr/local/kafka
export JAVA_HOME=/usr/local/mysql/enterprise/agent/java
export PATH=$PATH:$CONFLUENT_HOME/bin:$JAVA_HOME/bin
```
### Start Kafka
```
confluent local start
```
You should see kafka is started
```
Starting zookeeper
zookeeper is [UP]
Starting kafka
kafka is [UP]
Starting schema-registry
schema-registry is [UP]
Starting kafka-rest
kafka-rest is [UP]
Starting connect
connect is [UP]
Starting ksql-server
ksql-server is [UP]
```







