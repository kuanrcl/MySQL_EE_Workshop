# Elasticsearch with MySQL
Elasticsearch is a very popular full-text search engine for documents and data. We will use Elasticsearch to create index on JSON data stored in MySQL
## Install Elasticsearch
First of all, download and install Elasticsearch from 
https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started-install.html. Download the linux binaries.
```
cd /opt
tar zxvf /tmp/elasticsearch-7.6.1-linux-x86_64.tar.gz
sudo ln -s /opt/elasticsearch-7.6.1 /usr/local/elastic
export PATH=$PATH:/usr/local/elastic/bin
```
Start Elasticsearch
```
elasticsearch
```
Elasticsearch will take sometime to start depending on your VM. Once started, you can check the status
```
curl -X GET "localhost:9200/_cat/health?v&pretty"
epoch     timestamp cluster status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1584347399 08:29:59 elasticsearch yellow    1      1     1   1    0    0        1             0                  -                 50.0%
```
## Index some documents
Download the following accounts.json document from 
https://github.com/elastic/elasticsearch/blob/master/docs/src/test/resources/accounts.json?raw=true
Index the JSON documents (2000 docs)
```
cd /opt/download/lab/elastic
curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?pretty&refresh" --data-binary "@accounts.json"

curl "localhost:9200/_cat/indices?v"
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank  QtwKIJOCSp2HVb4M0iMNiw   1   1       1000            0    426.9kb        426.9kb
```

## Start searching
### Get all documents
```
 curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
'
```
### Get 10 docs
```
 curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ],
  "from": 10,
  "size": 10
}
'
```
### Using search criteria
Address field
```
curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "address": "mill lane" } }
}
'
```
Age and state
```
 curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
'
```
## Load the JSON docs into MySQL
Now we want to import the same accounts.json into MySQL
### Create the elastic database
mysql>
```
create database elastic;
\q
```
### Import the accounts.json into elastic database
We need to massage the accounts.json file before loading into the MySQL table. For each of the json in the accounts.json, there is a correspoinding {index:value}, we need to remove this from the input file
```
vi accounts.json
g:/"index"/d
w accounts_noindex.json
```
Copy the file accounts_noindex.json to /home/mysql/data/lab02/elastic
Add the secure_file_priv in my1.cnf
```
secure_file_priv=''
```

Next, create the table in MySQL
mysql>
```
use elastic;
create table accounts (id int not null auto_increment, doc json not null, primary key (id));
load data infile 'accounts_noindex.json' into table accounts(doc);
```
