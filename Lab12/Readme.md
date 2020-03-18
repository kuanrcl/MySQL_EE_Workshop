# Using Confluent Kafka with MySQL
## Install Confluent Kafka
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

## Configure Kafka Connect to MySQL
MySQL JDBC driver should be downloaded and installed

### Install mysql connector for java
Download the latest JDBC driver from https://dev.mysql.com/downloads/connector/j/
Unzip the file and make sure the driver is also at confluent jdbc directory
``` 
cd /opt
unzip /tmp/mysql-connector-java-8.0.19.zip
cd $CONFLUENT_HOME/share/java/kafka-connect-jdbc
ln -s /opt/mysql-connector-java-8.0.19/mysql-connector-java-8.0.19.jar .
```

### Start Kafka
Once the MySQL JDBC driver is installed, Kafka connect will load the MySQL JDBC driver during startup
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
## Prepare MySQL database
Create a simple table for testing
mysql>
```
create database ryan;
use ryan;
create table foobar \
(c1 int, \
c2 varchar(255), \
create_ts timestamp DEFAULT CURRENT_TIMESTAMP, \
update_ts timestamp DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP );
insert into foobar (c1,c2) values(1,'foo');
\q
```
## Configure MySQL Connection
```
## Configure MySQL conenction
We will create a simple JDBC connection properties to test Kafka connection to MySQL
### Edit "kafka-connect-jdbc-source.json"
```
cd /opt/download/lab/kafka
cat >> ./kafka-connect-jdbc-source.json << EOF
{
        "name": "jdbc_source_mysql_foobar_01",
        "config": {
                "_comment": "The JDBC connector class. Don't change this if you want to use the JDBC Source.",
                "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
                "_comment": "How to serialise the value of keys - here use the Confluent Avro serialiser. \
                Note that the JDBC Source Connector always returns null for the key ",
                "key.converter": "io.confluent.connect.avro.AvroConverter",
                "_comment": "Since we're using Avro serialisation, we need to specify the Confluent schema registry \
                at which the created schema is to be stored. NB Schema Registry and Avro serialiser are both part of \
                Confluent Platform.",
                "key.converter.schema.registry.url": "http://localhost:8081",
                "_comment": "As above, but for the value of the message. Note that these key/value serialisation settings \
                can be set globally for Connect and thus omitted for individual connector configs to make them shorter and clearer",
                "value.converter": "io.confluent.connect.avro.AvroConverter",
                "value.converter.schema.registry.url": "http://localhost:8081",
                "_comment": " --- JDBC-specific configuration below here  --- ",
                "_comment": "JDBC connection URL. This will vary by RDBMS. Consult your manufacturer's handbook for more information",
                "connection.url": "jdbc:mysql://localhost:3306/ryan?user=root&password=mysql",
                "_comment": "Which table(s) to include",
                "table.whitelist": "foobar",
                "_comment": "Pull all rows based on an timestamp column. You can also do bulk or incrementing column-based \
                extracts. For more information, see \
                http://docs.confluent.io/current/connect/connect-jdbc/docs/source_config_options.html#mode",
                "mode": "timestamp",
                "_comment": "Which column has the timestamp value to use?  ",
                "timestamp.column.name": "update_ts",
                "_comment": "If the column is not defined as NOT NULL, tell the connector to ignore this  ",
                "validate.non.null": "false",
                "_comment": "The Kafka topic will be made up of this prefix, plus the table name  ",
                "topic.prefix": "mysql-"
        }
}
EOF
```
### Load the JDBC connection 
```
cd $CONFLUENT_HOME
bin/confluent local load jdbc_source_mysql_foobar_01 -- -d /opt/download/kafka/kafka-connect-jdbc-source.json
```
You should 






