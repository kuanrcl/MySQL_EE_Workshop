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

Need to transform the downloaded json docs to multiple documents by looking for this pattern ``}}},{"id"`` in the doc

```
{"id":112,"url":"http://www.tvmaze.com/shows/112/south-park","name":"South Park","type":"Animation","language":"English","genres":["Comedy"],"status":"Running","runtime":30,"premiered":"1997-08-13","officialSite":"http://southpark.cc.com","schedule":{"time":"22:00","days":["Wednesday"]},"rating":{"average":8.6},"weight":97,"network":{"id":23,"name":"Comedy Central","country":{"name":"United States","code":"US","timezone":"America/New_York"}},"webChannel":null,"externals":{"tvrage":5266,"thetvdb":75897,"imdb":"tt0121955"},"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_portrait/0/935.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/0/935.jpg"},"summary":"<p><b>South Park</b> is an adult comedy animation show centred around 4 children in the small town of south park. Its humour is often dark involving satirical elements and mocking current real-life events.</p>","updated":1580329363,"_links":{"self":{"href":"http://api.tvmaze.com/shows/112"},"previousepisode":{"href":"http://api.tvmaze.com/episodes/1750710"}},"_embedded":{"episodes":[{"id":7919,"url":"http://www.tvmaze.com/episodes/7919/south-park-1x01-cartman-gets-an-anal-probe","name":"Cartman Gets an Anal Probe","season":1,"number":1,"airdate":"1997-08-13","airtime":"22:00","airstamp":"1997-08-14T02:00:00+00:00","runtime":30,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/75/188958.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/75/188958.jpg"},"summary":"<p>Aliens wreak havoc in the Rockies, first by subjecting Cartman to an anal probe, and then by abducting Kyle's baby brother.</p>","_links":{"self":{"href":"http://api.tvmaze.com/episodes/7919"}}},{"id":7920,"url":"http://www.tvmaze.com/episodes/7920/south-park-1x02-weight-gain-4000","name":"Weight Gain 4000","season":1,"number":2,"airdate":"1997-08-20","airtime":"22:00","airstamp":"1997-08-21T02:00:00+00:00","runtime":30,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/75/188957.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/75/188957.jpg"},"summary":"<p>Cartman tries to slim down when he learns a TV talk-show host is coming to town to give him an award, but he bulks up instead. Meanwhile, Mr. Garrison hatches a plan to murder the visiting celeb.</p>","_links":{"self":{"href":"http://api.tvmaze.com/episodes/7920"}}},{"id":7921,"url":"http://www.tvmaze.com/episodes/7921/south-park-1x03-volcano","name":"Volcano","season":1,"number":3,"airdate":"1997-08-27","airtime":"22:00","airstamp":"1997-08-28T02:00:00+00:00","runtime":30,"image":{"medium":"http://static.tvmaze.com/uploads/images/medium_landscape/45/112943.jpg","original":"http://static.tvmaze.com/uploads/images/original_untouched/45/112943.jpg"},

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
4. download lefred PHP XDevAPI application
```
git clone https://github.com/lefred/mysql-docstore-php-example.git
php -S 192.168.56.106:8000
```





