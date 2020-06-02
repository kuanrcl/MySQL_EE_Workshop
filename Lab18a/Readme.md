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
\js
\c root:mysql@localhost:33060
\u docstore
var r=db.getCollection('restaurants')
r.find().limit(2)
```

## More examples
```
r.find.fields("name", "cuisine").limit(2)
r.find("cuision='Bakery'").fields("name", "cuisine").limit(2)
r.find("cuisine in ('Turkish', 'Italian')").fields("name", "cuisine").limit(10)
r.find("cuisine='Italian' and borough!='Manhattan'").fields("name", "cuisine", "borough").limit(2)

\sql
with cte1 as (select doc->>"$.name" as name, doc->>"$.cuisine" as cuisine, (select avg(score) from json_table(doc, "$.grades[*]" columns (score int path "$.score")) as r) as avg_score from restaurants) select *, rank() over (partition by cuisine order by avg_score desc) as 'rank' from cte1 order by 'rank', avg_score desc limit 10;
```

## TVShow examples

* Download tvshow json document from https://github.com/jdorfman/awesome-json-datasets#tv-shows
* Need to transform the downloaded json file because the data is saved as 1 huge json document
* Save one of the tvshow, for example, homeland.json
```
sed 's/}}},{"id"/}}}{"id"/g' homeland.json.0 > homeland.json.1
```
* Strip off the square bracket enclosing the entire json doc, ie the "\[" at position 0, and the last "\]" at the end of file
* import the json doc
```
mysqlsh>
util.importJson("/opt/lab/db/window/homeland.json.1", {schema: "docstore", collection: "homeland", convertBsonOid:true})
db.homeland.find().limit(1)
```

 

\u tvshow
db.getCollection('homeland').find().fields("_embedded.episodes[*].season","_embedded.episodes[*].name").sort('_embedded.episodes[*].season').limit(1)
```

## Using mysqlxdev API (Ubuntu 7.8)
1. install development tools
```
apt install build-essential libprotobuf-dev libboost-dev openssl protobuf-compiler liblz4-tool zstd
apt install php7.2-cli php7.2-dev php7.2-mysql php7.2-pdo php7.2-xml
```
2. install mysql-server
```
wget -c https://repo.mysql.com//mysql-apt-config_0.8.13-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.13-1_all.deb 
sudo apt update
sudo apt-get install mysql-server
```
3. install mysqlxdevapi (requires boost and protobuf libraries)
```
sudo pecl install mysql_xdevapi
echo "extension=mysql_xdevapi.so" > /etc/php/7.2/mods-available/mysql_xdevapi.ini
phpenmod -v 7.2 -s ALL mysql_xdevapi
php -m |grep mysql
mysql_xdevapi
mysqli
mysqlnd
pdo_mysql
```





