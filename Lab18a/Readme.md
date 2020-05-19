# MySQL Document Store

## Install Mongodb
1. Install mongodb community

```
sudo vi /etc/yum.repo.d/mongodb-org-4.2.repo
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
```
2. Install and start mongodb
```
sudo yum install mongodb-org
sudo systemctl start mongod
```
3. Import the ''restaurants'' dataset
```
wget https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json
mongoimport --db test --collection restaurants --drop --file ./primer-dataset.json
```
4. Export the restaurants dataset
```
mongoexport --collection=restaurants --db=test > from_mongo.json
```
Results:
```
2020-05-06T13:46:22.759+0800    connected to: mongodb://localhost/
2020-05-06T13:46:23.763+0800    [........................]  test.restaurants
2020-05-06T13:46:24.761+0800    [........................]  test.restaurants
2020-05-06T13:46:25.762+0800    [........................]  test.restaurants
2020-05-06T13:46:26.771+0800    [#######.................]  test.restaurants
2020-05-06T13:46:27.764+0800    [#######.................]  test.restaurants
2020-05-06T13:46:28.761+0800    [#######.................]  test.restaurants
2020-05-06T13:46:29.762+0800    [###############.........]  test.restaurants
2020-05-06T13:46:30.761+0800    [###############.........]  test.restaurants
2020-05-06T13:46:31.477+0800    [########################]  test.restaurants
2020-05-06T13:46:31.477+0800    exported 25359 records
```

## Import the restaurants dataset into MySQL

```
mysqlsh
\py
s=shell.connect('root:mysql@localhost:33060')
db=s.create_schema('docstore');
\js
\u docstore
util.importJson("./from_mongo.json", {schema: "docstore", collection: "restaurants", convertBsonOid: true});
```
Results:
```
Importing from file "./restaurant_mongo.json" to collection `docstore`.`restaura                                     nts` in MySQL Server at localhost:33060

.. 25359.. 25359
Processed 12.44 MB in 25359 documents in 14.5641 sec (1.74K documents/s)
Total successfully imported documents 25359 (1.74K documents/s)
```
Check the imported documents:
```
mysqlsh

![Install](img/INS1.png)
