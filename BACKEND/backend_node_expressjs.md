# **BASIC**
# =======================================================
Express is a web framework for Node.js. It is the first successful Node.js framework and still the most used. It is very minimalist and can be extended using its middleware system.
Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications. It is an open source framework developed and maintained by the Node.js foundation.

# **COMPARISON**
# =======================================================
Unlike its competitors like Rails and Django, which have an opinionated way of building applications, express has no "best way" do something. It is very flexible and pluggable.


# **DATABASE - MongoDB and Mongoose**
# =======================================================
MongoDB and Mongoose
MongoDB is an open-source, document database designed for ease of development and scaling. Well use this database to store data.
Mongoose is a client API for node.js which makes it easy to access our database from our express application.


# **INSTALLATION**
# =======================================================
node and npm have to be installed already.

node --version
npm --version


##### Node Package Manager (NPM)
The npm Registry is a public collection of packages of open-source code for Node.js, front-end web apps, mobile apps, robots, routers, and countless other needs of the JavaScript community. npm allows us to access all these packages and install them locally. 

Install Packages via the 2 methods:
By default packages are installed locally
**Globally**: This method is generally used to install development tools and CLI based packages. To install a package globally, use:
```
npm install -g <package-name>
```

**Locally**: This method is generally used to install frameworks and libraries. A locally installed package can be used only within the directory it is installed. To install a package locally use the same command as above without the -g flag.
```
npm install <package-name>
```


##### Installing
mkdir new_project
cd new_project

creates package.json file where all the packages are listed.
npm init

The --save flag can be replaced by -S flag. This flag ensures that express is added as a dependency to our package.json file. This has an advantage, the next time we need to install all the dependencies of our project we just need to run the command npm install and it’ll find the dependencies in this file and install them for us.
```
npm install --save express
```

We also isntall nodemon.
What this tool does is, it restarts our server as soon as we make a change in any of our files, otherwise we need to restart the server manually after each file modification.
```
npm install -g nodemon
```



# **BASIC FUNCTIONING**
# =======================================================
create index.js in the root directory
Imports express is picked up from the node_modules directory automatically and is avialble in the root folder. We use it to create an application and assign it to var app.

```
var express = require('express');
var app = express();
```

**app.get(route, callback)** - This function tells what to do when a get request at the given route is called. The callback function has 2 parameters, request(req) and response(res).
**res.send()** - This function takes an object as input and it sends this to the requesting client. Here we are sending the string "Hello World!".

```
app.get('/', function(req, res){
    res.send("Hello world!");
});
```

app.listen(port, [host], [backlog], [callback]]) - This function binds and listens for connections on the specified host and port. Port is the only required parameter here.
app.listen(3000);



# **ROUTING**
# =======================================================

##### Router Methods
Following are the routing methods:
**get, post, put, head, delete, options, trace, copy, lock, mkcol, move, purge, propfind, proppatch, unlock, report, mkactivity, checkout, merge, m-search, notify, subscribe, unsubscribe, patch, search, and connect.**
Example with multiple methods
```
app.get('/hello', function(req, res){
  res.send("Hello World!");
});

app.post('/hello', function(req, res){
  res.send("You just called the post method at '/hello'!\n");
});
```

Testing with POST method
```
curl -X POST "http://localhost:3000/hello"
```

###### Special Method all
Special method for handling all types of http requests to a route
This method is used for loading middleware functions at a path for all request methods.
```
app.all('/test', function(req, res){
  res.send("HTTP method doesn't have any effect on this route!");
});
```
###### Rendering vies from thr router line
```
app.get('/components', function(req, res){
    res.render('content');
});
```

###### Using Router for managing multiple routes
create a new file named anything. Say things.js for routes related to things ressource.
*things.js*
```
var express = require('express');
var router = express.Router();
router.get('/', function(req, res){
  res.send('GET route on things.');
});
router.post('/', function(req, res){
  res.send('POST route on things.');
});
//export this router to use in our index.js
module.exports = router;
```
Then **import** this in the index.js or the main server file where exress is defined. Both index.js and things.js should be in same directory
*index.js*
```
var express = require('express');
var app = express();
var things = require('./things.js'); 

app.use('/things', things); 
app.listen(3000);
```
You should define your routes relating to an entity in a single file and include it using above method in your index.js file.

###### Resourced routes
```
app.get('/things/:name/:id', function(req, res){
    res.send('id: ' + req.params.id + ' and name: ' + req.params.name);
});

app.get('/:id', function(req, res){
    res.send('The id you specified is ' + req.params.id);
});

app.get('/things/:name/:id', function(req, res){
    res.send('id: ' + req.params.id + ' and name: ' + req.params.name);
});
```

###### Regex matching
```
app.get('/things/:id([0-9]{5})', function(req, res){
    res.send('id: ' + req.params.id);
});

app.get('*', function(req, res){
    res.send('Sorry, this is an invalid URL.');
});
```

# **EXPRESS GENERATOR**
# =======================================================
install the express generator globally
npm install express-generator -get

express folder_name
OR
express .

Then to install dependencies and run the app.
npm install
npm start

List of files that are generated:


# **MIDDLEWARE**
# =======================================================
var express = require('express');
var app = express();

###### Middleware function to log request protocol
```
app.use('/things', function(req, res, next){
  console.log("A request for things received at " + Date.now());
  next();
});
```

###### Route handler that sends the response
```
app.get('/things', function(req, res){
  res.send('Things'); 
});
app.listen(3000);
```

###### General Use case
The general use case is that before the request goes to routing, we may add some custom process to be done.

```
var express = require('express');
var app = express();
```

###### First middleware before response is sent
```
app.use(function(req, res, next){
  console.log("Start");
  next();
});
```
Route handler
```
app.get('/', function(req, res, next){
  res.send("Middle");
  next();
});
```

###### After the request flow comes from the routing.
```
app.use('/', function(req, res){
  console.log('End');
});
app.listen(3000);
```


###### Third Party Middlewares - Body Parser
This is used to parse the body of requests which have payloads attached to them. Body parser is a middleware to handle POST data in Express. In express 4 you have to manually install and mention the middleware. You can install by typing.

```
npm install --save body-parser

var bodyParser = require('body-parser');
//To parse URL encoded data
app.use(bodyParser.urlencoded({ extended: false }))
//To parse json data
app.use(bodyParser.json())
```

###### Third Party Middlewares - Cookie Parser
Cookie parser is a middleware to handle cookie in Express. In express 4 you have to manually install and mention the middleware. You can install by typing.

```
npm install --save cookie-parser
app.use(cookieParser())
```

###### Third Party Middlewares - Cookie Parser
Express Session is a middleware to handle Session in Express. In express 4 you have to manually install and mention the middleware. You can install by typing.
```
npm install --save express-session
app.use(session({ secret: '$#%!@#@@#SSDASASDVV@@@@', key: 'sid'}));
```

# USEFUL PACKAGES
# =======================================================
body-parser – To handle POST data.
cookie-parser – To manage cookies.
morgan – for dev purpose only
ejs- Template engine
express-session – For handling Session.
nodemailer – To handle emails.

# TEMPLATING
# =======================================================
Setting the templating engine - from HTML, Pug or EJS
```
app.set('view engine', 'ejs');
```
Then tell express from where to deliver the front end file.
```
app.set('views', __dirname + '/views');
```


# TEMPLATING - PUG
# =======================================================
Pug is a templating engine for express. Templating engines are used to remove the cluttering of our server code with HTML, concatenating strings wildly to existing HTML templates.
https://www.tutorialspoint.com/expressjs/expressjs_templating.htm
Pug is a very powerful templating engine which has a variety of features including filters, includes, inheritance, interpolation, etc. There is a lot of ground to cover on this.
Pug(earlier known as Jade) is a terse language for writing HTML templates. It:
Produces HTML
Supports dynamic code
Supports reusability (DRY)
It is one of the most popular templating language used with express.

Install pug
```
npm install --save pug
```
Set pug as the templating engine for the app inside index.js file
```
app.set('view engine', 'pug');
app.set('views','./views');
```
Create first pug view
```
doctype html
html
    head
        title="Hello Pug"
    body
        p.greetings#people Hello World!
```
Render the pug using the following
```
app.get('/first_template', function(req, res){
    res.render('first_view');
});
```
Pug generates the following html code
```
<!DOCTYPE html>
<html>
<head>
<title>Hello Pug</title>
</head>
<body>
<p class="greetings" id="people">Hello World!</p>
</body>
</html>
```

div.container.column.main#division(width="100",height="100")
This line of code, get converted to:
```
<div class="container column main" id="division" width="100" height="100"></div>
```



# TEMPLATING - EJS
# =======================================================



# STATIC FILES
# =======================================================
Static files are files that clients download as they are from the server. Create a new directory, public. Express, by default doesn't allow you to serve static files. You need to enable it using the following built-in middleware.
You have to tell Express from where it should deliver static files. Static files are images, JavaScript library, CSS files etc. You can specify by using.

**Note**: Express looks up the files relative to the static directory, so the name of the static directory is not part of the URL.
**Urls:** Using the following methods the assets are loaded directly at the root path. For example a file some.png is loaded as www.website.com/some.png.
Use the following line
```
// Absolute
app.use(express.static(__dirname + '/folder_name'));

// Relative
app.use(express.static('folder_name'));
```
###### Multiple Asset Directories
```
var express = require('express');
var app = express();

app.use(express.static('public'));
app.use(express.static('images'));

app.listen(3000);
```
###### Virtual Path Prefix
Use the following to add virtual path prefix to static files being served.
The following adds "/static" to all the static files in the "public" directory www.website.com/static/some.png
```
app.use('/static', express.static('public'));
```

# FORM DATA
# =======================================================
Form data sent to an api hosted is sent in JSON format. For parsing the JSON we need a body parser that parses the JSON passed to the POST api.
To process form data first install the **body-parser**(for parsing JSON and url-encoded data) and **multer**(for parsing multipart/form data) middleware.
```
npm install --save body-parser multer
```

**index.js (backend code)**
```
var express = require('express');
var bodyParser = require('body-parser');
var multer = require('multer');
var upload = multer(); 
var app = express();

app.set('view engine', 'pug');
app.set('views', './views');

// Include all the middlewares
// for parsing application/json
app.use(bodyParser.json());
// for parsing application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: true }));
// for parsing multipart/form-data
app.use(upload.array());
app.use(express.static('public'));

app.post('/', function(req, res){
    // Printing the body of the post request for now
    console.log(req.body);
    res.send("recieved your request!");
});

app.listen(3000);
```

**index.pug (Front End)**
```
html
    head
        title Form Tester
    body
        form(action="/", method="POST")
            div
                label(for="say") Say: 
                input(name="say" value="Hi")
            br
            div
                label(for="to") To: 
                input(name="to" value="Express forms")
            br
            button(type="submit") Send my greetings
```

# COOKIES
# =======================================================
Cookies are simple, small files/data that are sent to client with a server request and stored on the client side. Every time the user loads the website back, this cookie is sent with the request.
We will be doing the following:
*    Session management
*    Personalization(Recommendation systems)
*    User tracking

To use cookies with express, we need the cookie-parser middleware.
```
npm install --save cookie-parser
```
###### Using CookieParser
cookieParser is a middleware which parses cookies attached to the client request object. To use it, we will require it in our index.js file and use it in the same way we use other middleware. We are going to use it on every route.

```
var cookieParser = require('cookie-parser');
app.use(cookieParser());
```

###### Cathcing Cookie
cookie-parser parses Cookie header and populates req.cookies with an object keyed by the cookie names. To set a new cookie lets define a new route in your express app like: 

```
var express = require('express');
var app = express();

app.get('/', function(req, res){
    res.cookie('name', 'express').send('cookie set'); //Sets name=express
});

app.listen(3000);
```

###### Viewing Cookie in browser
To check if your cookie is set or not, just go to your browser, fire up the console, and enter:

```
console.log(document.cookie);
```

###### Viewing Cookie in Express
The browser also sends back cookies every time is queries the server. To view cookies from your server, on the server console in a route, add the following code to that route:

```
console.log('Cookies: ', req.cookies);
```

###### Adding Cookies
You can add cookies that expire. To add a cookie that expires, just pass an object with property 'expire' set to the time when you want it to expire. For example,

```
//Expires after 360000 ms from the time it is set.
res.cookie(name, 'value', {expire: 360000 + Date.now()}); 
```

Another way to set expiration time is using 'maxAge' property. Using this property, we can provide relative time instead of absolute time. An example of this method:

```
//This cookie also expires after 360000 ms from the time it is set.
res.cookie(name, 'value', {maxAge: 360000});
```

###### Deleting Cookie
To delete a cookie, use the clearCookie function. For example, if you need to clear a cookie named foo, use the following code:

```
var express = require('express');
var app = express();

app.get('/clear_cookie_foo', function(req, res){
    clearCookie('foo');
    res.send('cookie foo cleared');
});

app.listen(3000);
```

   
# SESSIONS
# =======================================================

# AUTHENTICATION
# =======================================================

# DEBUGGING
# =======================================================

# PERFORMANCE TESTING
# =======================================================

# PRODUCTION INSTALLATION
# =======================================================

# WEBSITES
https://strongloop.com/strongblog/category/express
http://www.hacksparrow.com/category/express-js
https://codeforgeek.com/2014/10/express-complete-tutorial-part-1/
http://node-tricks.com/category/express/
https://www.rosehosting.com/blog/tag/express/