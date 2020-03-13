# Node.js with MySQL Document Store
We will use Visual Studio Code (VS Code) on Windows to develop a simple Node.js application using MySQL. 

## Install Visual Studio Code (with Node.js)
First, download and install Visual Studio Code from https://code.visualstudio.com/
Run VS Code, and create a new project "MySQL JSON" in your "C:\Users\<name>\.vscode" directory

### Install essential Node.js modules
Next, open a "Terminal"
```
npm -init
npm install @mysql/xdevapi --save --save-exact
npm install express --save
npm install body-parser --save
npm install express-validation --save
npm install express-handlebars --save
```
## Create a table with json data type
Connect to a MySQL instance
mysql>
```
create database products;
use products;
create table items (c1 json not null);
insert into items values ('{"name":"apple", "price":"1"}');
select * from items;
```
## Create Node.js application with MySQL XDevAPI
Create a new file called "index.js"
```
var mysqlx = require('@mysql/xdevapi');
const express = require('express');
const app = express();
const router = express.Router();
const bodyParser = require('body-parser');
const doc = [];

// load the handlebars module
const expHBS = require('express-handlebars');

// Sets our app to use the handlebars engine
app.set('view engine', 'handlebars');
app.engine('handlebars', expHBS());

app.use((req, res, next) => {
    console.log('middleware works!');
    next();
});

app.use(bodyParser.urlencoded({
    extended: true
}));
app.use(bodyParser.json());
app.use('/', router);

router.route('/').get((req, res) => {
    
    
    mysqlx.getSession('mysqlx://demo:password@192.168.56.41:33060')
    .then(session => {
        const collection = session.getSchema('products').getCollection('items');

        return collection.find().execute();
        })
        .then(result => {
                
                res.render('home', {
                    pageTitle: 'Welcome to shithole',
                    products: result.fetchAll()
                });
        })
        .catch(err=>{
            console.log(err);
        })  
});

router.route('/newproduct').post((req, res) =>{
    
    mysqlx.getSession('mysqlx://demo:password@192.168.56.41:33060/products')
    .then(session => {
        return session.getSchema('products').getCollection('items');
    })
    .then(collection => {
        return collection.add({ name: req.body.name, price: req.body.price }).execute()
    })
    .catch(err=>{
        console.log(err);  
    });
    
    res.send('Data saved successfully');
});

app.listen(3002, () => {
    console.log('Server started at Port 3002');
});
module.exports = app;
```

### Create handlebars called "home.handlebars"
First, create a folder "views"

![views](img/M2.png)

Create the html tags in "home.handlebars"
```
<h1>Welcome</h1>
<ul>
    {{#each products}}
        <li>{{ name }} - ${{ price }}</li>
    {{/each}}
</ul>
<form method="post" action="/newproduct">
    Name: <input type="text" name="name"><br>
    Price: <input type="text" name="price"><br>
    <button type="submit">Submit</button>
</form>
```

### Run the sample application
Open a terminal
```
nodemon
```
![node](img/M3.png)

### Test the application
Open a browser to "http://localhost:3002"
Enter some values, voila!

![Test](img/M4.png)

## Check the MySQL database
mysql>
```
use products;
select * from items;
+---------------------------------------------------------------------------+
| doc                                                                       |
+---------------------------------------------------------------------------                                       |
| {"_id": "00005e5420230000000000000006", "name": "Racket", "price": "100"} |
| {"_id": "00005e5420230000000000000007", "name": "iPad", "price": "2000"}  |
| {"_id": "00005e5420230000000000000008", "name": "Apple", "price": "1"}    |
| {"_id": "00005e5420230000000000000009", "name": "Orange", "price": "1"}   |
+---------------------------------------------------------------------------+
```






