# Using XDEVAPI with PHP and mongodb

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

5. Simple mongodb commmands

```
mongo
show dbs
use docstore
show collections
homeland.find().limit(1).pretty()
db.homeland.find("season=1").pretty()
db.homeland.find({"season": 1})
db.restaurants.find({}, {name:1, grades:1}).limit(1).pretty()
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
Importing from file "./restaurant_mongo.json" to collection `docstore`.`restaurants` in MySQL Server at localhost:33060

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
r.find().fields("name", "cuisine").limit(2)
r.find("cuisine='Bakery'").fields("name", "cuisine").limit(2)
r.find("cuisine in ('Turkish', 'Italian')").fields("name", "cuisine").limit(10)
r.find("cuisine='Italian' and borough!='Manhattan'").fields("name", "cuisine", "borough").limit(2)

\sql
with cte1 as (select doc->>"$.name" as name, doc->>"$.cuisine" as cuisine, (select avg(score) from json_table(doc, "$.grades[*]" columns (score int path "$.score")) as r) as avg_score from restaurants) select *, rank() over (partition by cuisine order by avg_score desc) as 'rank' from cte1 order by 'rank', avg_score desc limit 10;
```

## TVShow examples

* Download tvshow json document from https://github.com/jdorfman/awesome-json-datasets#tv-shows
* Need to transform the downloaded json file because the data is saved as 1 huge json document
* Save one of the tvshow, for example, homeland.json

For example, Game of Thrones episodes, show id = 82  
```
http://api.tvmaze.com/shows/82/episodes
save all the episodes in 1 json file
```

Need to transform the downloaded json docs to multiple documents by looking for this pattern ``}}},{"id"`` in the doc

```
[{"id":4952,"url":"http://www.tvmaze.com/episodes/4952/game-of-thrones-1x01-winter-is-coming","name":"Winter is Coming","season":1,"number":1,"airdate":"2011-04-17","airtime":"21:00","airstamp":"2011-04-18T01:00:00+00:00","runtime":60,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/1/2668.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/1/2668.jpg"},"summary":"<p>Lord Eddard Stark, ruler of the North, is summoned to court by his old friend, King Robert Baratheon, to serve as the King's Hand. Eddard reluctantly agrees after learning of a possible threat to the King's life. Eddard's bastard son Jon Snow must make a painful decision about his own future, while in the distant east Viserys Targaryen plots to reclaim his father's throne, usurped by Robert, by selling his sister in marriage.</p>","_links":{"self":{"href":"http://api.tvmaze.com/episodes/4952"}}},{"id":4953,"url":"http://www.tvmaze.com/episodes/4953/game-of-thrones-1x02-the-kingsroad","name":"The Kingsroad","season":1,"number":2,"airdate":"2011-04-24","airtime":"21:00","airstamp":"2011-04-25T01:00:00+00:00","runtime":60,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/1/2669.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/1/2669.jpg"},"summary":"<p>An incident on the Kingsroad threatens Eddard and Robert's friendship. Jon and Tyrion travel to the Wall, where they discover that the reality of the Night's Watch may not match the heroic image of it.</p>","_links":{"self":{"href":"http://api.tvmaze.com/episodes/4953"}}},{"id":4954,"url":"http://www.tvmaze.com/episodes/4954/game-of-thrones-1x03-lord-snow","name":"Lord Snow","season":1,"number":3,"airdate":"2011-05-01","airtime":"21:00","airstamp":"2011-05-02T01:00:00+00:00","runtime":60,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/1/2671.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/1/2671.jpg"},"summary":"<p>Jon Snow attempts to find his place amongst the Night's Watch. Eddard and his daughters arrive at King's Landing.</p>","_links":{"self":{"href":"http://api.tvmaze.com/episodes/4954"}}},{"id":4955,"url":"http://www.tvmaze.com/episodes/4955/game-of-thrones-1x04-cripples-bastards-and-broken-things","name":"Cripples, Bastards, and Broken Things","season":1,"number":4,"airdate":"2011-05-08","airtime":"21:00","airstamp":"2011-05-09T01:00:00+00:00","runtime":60,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/1/2673.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/1/2673.jpg"},"summary":"<p>Tyrion stops at Winterfell on his way home and gets a frosty reception from Robb Stark. Eddard's investigation into the death of his predecessor gets underway.</p>","_links":{"self":{"href":"http://api.tvmaze.com/episodes/4955"}}},{"id":4956

sed 's/}}},{"id"/}}}{"id"/g' homeland.json.0 > homeland.json.1
```

* Strip off the square bracket enclosing the entire json doc, ie the "\[" at position 0, and the last "\]" at the end of file
* import the json doc

```
mysqlsh>
util.importJson("/opt/lab/db/window/homeland.json.1", {schema: "docstore", collection: "homeland", convertBsonOid:true})
db.homeland.find().limit(1)
db.homeland.find("season=1").fields("name", "summary", "airdate").sort("number")
db.homeland.find("number=1").fields("name", "airdate", "season").sort("season")
db.homeland.find("number=1 AND season=1").fields("name", "airdate")

var hlEpisode = db.homeland.find("number = :episodeNum AND season = :seasonNum").fields("name", "airdate")
hlEpisode.bind('episodeNum', 1).bind('seasonNum', 1)
hlEpisode.bind('episodeNum', 1).bind('seasonNum', 7)

db.homeland.createIndex('idxSeasonEpisode', {fields: [{field: "$.season", type: "TINYINT UNSIGNED", required: true}, {field: "$.number", type: "TINYINT UNSIGNED", required: true}]})
 
db.homeland.createIndex('idxSummary', {fields: [{field: "$.summary", type: "TEXT(30)"}]})
db.homeland.createIndex('idxId', {fields: [{field: "$.id", type: "INT UNSIGNED"}], unique: true})

```

### SQL

```
select doc from homeland limit 1;
select doc->>"$.name", doc->>"$.summary", doc->>"$.airdate" from homeland where doc->>"$.season"=1 order by doc->>"$.number";
select doc->>"$.name", doc->>"$.airdate", doc->>"$.season" from homeland where doc->>"$.number"=1 order by doc->>"$.season";
select doc->>"$.name", doc->>"$.airdate" from homeland where doc->>"$.number"=1 and doc->>"$.season"=1;
select doc->>"$.season", count(doc->>"$.number") from homeland group by doc->>"$.season";
```


### OMITTED
the following example is only good if you import the tvshow json doc as is into MySQL

```
\u tvshow
db.getCollection('homeland').find().fields("_embedded.episodes[*].season","_embedded.episodes[*].name").sort('_embedded.episodes[*].season').limit(1)
```

## transaction

```
db.createCollection('my_coll1');
db.my_coll1.add({"title":"MySQL Document Store", "abstract":"SQL is now optional!", "code": "42"})
db.my_coll1.add("This is not a valid JSON document")
db.my_coll1.find()
db.my_coll1.add({"title":"Quote", "message": "Strive for greatness"})
db.my_coll1.modify("_id='00005c9514e60000000000000054'").unset("title")
db.my_coll1.remove("_id='00005c9514e60000000000000054'")

session.startTransaction()
db.my_coll1.find()
db.my_coll1.modify("_id = '00005ed5b7e10000000000000066'").unset("code")
db.my_coll1.add({"title":"Quote", "message": "Be happy, be bright, be you"})
db.my_coll1.find()
session.rollback()
db.my_coll1.find()
```
## Using mysqlxdev API (Oracle Linux 7.8)

**Best way to install xdevapi driver**
Use Remi Repo (https://blog.remirepo.net/pages/Config-en)

http://rpms.remirepo.net/enterprise/7/php74/x86_64/php-pecl-mysql-xdevapi-8.0.20-1.el7.remi.7.4.x86_64.rpm

```
# remirepo
sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
sudo yum-config-manager --enable remi-php74

# Oracle repo

# install php
sudo yum install -y oracle-php-release-el7 oracle-epel-release-el7 mysql-release-el7
sudo yum install php php-mysqlnd php-json 
sudo yum install php php-cli php-mysqlnd php-zip php-gd php-mcrypt php-mbstring php-xml php-json php-pear php-devel
sudo yum install boost-devel protobuf-devel
sudo yum install devtoolset-8 
sudo scl enable devtoolset-8 bash

# install mysql_xdevapi
sudo pecl install mysql_xdevapi
Installing '/usr/lib64/php/modules/mysql_xdevapi.so'
install ok: channel://pecl.php.net/mysql_xdevapi-8.0.20
configuration option "php_ini" is not set to php.ini location
You should add "extension=mysql_xdevapi.so" to php.ini

# vi /etc/php.d/40-mysql_xdevapi.ini
extension=mysql_xdevapi.so

```

## Using mysqlxdev API (Ubuntu 18.04)
1. install development tools
```
apt update
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
The pecl install takes a long time, it looks like it is compiling the mysqlx api
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
4. download lefred PHP XDevAPI application
```
git clone https://github.com/lefred/mysql-docstore-php-example.git
php -S 192.168.56.106:8000
```





