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

###### Testing with POST method
```
curl -X POST "http://localhost:3000/hello"
```

###### Load Router Methods in the app
```
var birds = require('./birds')

// ...

app.use('/birds', birds)
```

###### Special Method all
Special method for handling all types of http requests to a route
This method is used for loading middleware functions at a path for all request methods.
```
app.all('/test', function(req, res){
  res.send("HTTP method doesn't have any effect on this route!");
});
```

###### Rendering views from the router line
```
app.get('/components', function(req, res){
    res.render('content');
});
```

###### Route Handlers File (app.use) (express.Router)
Use the express.Router class to create modular, mountable route handlers. A Router instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.
The following example creates a router as a module, loads a middleware function in it, defines some routes, and mounts the router module on a path in the main app.
Say things.js for routes related to things ressource.
The most important bit, from the app file above, is
```
app.use('/cars', require('./cars'))
```
It loads the router with its routes and defines a prefix for all the routes loaded inside. The prefix part is optional (default is '/'). You could write for example
```
app.use(require('./cars'))
```
Then the server will respond only to requests from /brands and /models.

*things.js*
```
var express = require('express');
var router = express.Router();

// Home Page Route
router.get('/', function(req, res){
  res.send('GET route on things.');
});

// Home Page Post
router.post('/', function(req, res){
  res.send('POST route on things.');
});

//export this router to use in our index.js
module.exports = router;
```

Then **import** this in the index.js or the main server file where exress is defined. Both index.js and things.js should be in same directory. Here all the routes are scoped under **/things**
So we have routes like **/things/about, /things/some**
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

###### Request Parameters
```
app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
})
```

###### Response Methods
**res.download()** 	Prompt a file to be downloaded.
**res.end()** 	End the response process.
**res.json()** 	Send a JSON response.
**res.jsonp()** 	Send a JSON response with JSONP support.
**res.redirect()** 	Redirect a request.
**res.render()** 	Render a view template.
**res.send()** 	Send a response of various types.
**res.sendFile()** 	Send a file as an octet stream.
**res.sendStatus()** 	Set the response status code and send its string representation as the response body.

###### Chainable Routes
You can create chainable route handlers for a route path by using app.route(). Because the path is specified at a single location, creating modular routes is helpful, as is reducing redundancy and typos.
```
app.route('/book')
  .get(function (req, res) {
    res.send('Get a random book')
  })
  .post(function (req, res) {
    res.send('Add a book')
  })
  .put(function (req, res) {
    res.send('Update the book')
  })
```

###### Single File to manage routes
Every router can load other routers. This is very handy when you are organizing your app. You can even build a hierarchy of routers and routes, if you really need it.
To load them, you only need to load the controllers/index.js file. Moreover, it is an index file, so you don’t need to provide its name when requiring it, you only need the folder name (for example controllers/index.js).
```
app.use(require('./controllers'))
app.use(require('router.js'))
```

Content of controllers/index.js
```
var express = require('express');
var router = express.Router();

router.use('/animals', require('./animals'))
router.use('/cars', require('./cars'))

router.get('/', function(req, res) {
  res.send('Home page')
})

router.get('/about', function(req, res) {
  res.send('Learn about us')
})

module.exports = router
```

###### 404 not found route

Handling 500 Errors (server errors)

# **EXPRESS GENERATOR**
# =======================================================
install the express generator globally
```
npm install express-generator -get
```
All the available commands
```
express -h
```
Install express - scaffold command
```
express folder_name
OR
express .
```
The following creates an Express app named myapp in the current working directory:
```
express --view=jade myapp
```
Then to install dependencies and run the app.
```
npm install
npm start
```


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

###### Router Specific Middlewares
Middlewares in express are extremely useful. They can load sessions, extract useful data, put common headers and much more.

While most of the time you add middlewares for the entire app with app.use, sometimes it is also useful to have them only for some routes. In such cases you would usually add the middleware to every individual path definition.

```
var express = require('express')
  , router = express.Router()

// Middleware
function authorize(req, res, next) {
  if (req.user === 'farmer') {
   next()
  } else {
    res.status(403).send('Forbidden')
  }
}

// Domestic animals page
router.get('/domestic', authorize, function(req, res) {
  res.send('Cow, Horse, Sheep')
})

// Wild animals page
router.get('/wild', authorize, function(req, res) {
  res.send('Wolf, Fox, Eagle')
})

module.exports = router
```

In larger more complex apps, having to add the middleware to each individual route quickly becomes tedious. In such cases you can add the middleware directly to the router.

```
var express = require('express')
  , router = express.Router()

// Applying middleware to all routes in the router
router.use(function (req, res, next) {
  if (req.user === 'farmer') {
    next()
  } else {
    res.status(403).send('Forbidden')
  }
})

// Domestic animals page
router.get('/domestic', function(req, res) {
  res.send('Cow, Horse, Sheep')
})

// Wild animals page
router.get('/wild', function(req, res) {
  res.send('Wolf, Fox, Eagle')
})

module.exports = router
```

The middlware will be executed only for all routes defined in this router and only for them. This can be used for defining whole controller authorization or making some input data transformation which will be then used everywhere inside the router or another common task.
# USEFUL PACKAGES
# =======================================================
body-parser – To handle POST data.
cookie-parser – To manage cookies.
morgan – for dev purpose only
ejs- Template engine
express-session – For handling Session.
nodemailer – To handle emails.

# TEMPLATING - RENDERING VIEWS AND ROUTES
# =======================================================
Setting the templating engine - from HTML, Pug or EJS
```
app.set('view engine', 'ejs');
```
Then tell express from where to deliver the front end file.
```
app.set('views', __dirname + '/views');
```
Render view via a route
```
app.get('/first_template', function(req, res){
    res.render('first_view');
});
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
###### Create first pug view
```
doctype html
html
    head
        title="Hello Pug"
    body
        p.greetings#people Hello World!
```
###### Render the pug using the following
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

###### Passing values to templates
```
var express = require('express');
var app = express();

app.get('/dynamic_view', function(req, res){
    res.render('dynamic', {
        name: "TutorialsPoint", 
        url:"http://www.tutorialspoint.com"
    });
});

app.listen(3000);
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
* Session management
* Personalization(Recommendation systems)
* User tracking

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
https://codeforgeek.com/2014/10/express-complete-tutorial-part-4/
Because HTTP is stateless, in order to associate a request to any other request, you need a way to store user data between HTTP requests. Cookies and URL parameters are both suitable ways to transport data between client and server. But they are both readable and on the client side. Sessions solve exactly this problem. You assign the client an ID and it makes all further requests using that ID. Information associated with the client is stored on the server linked to this ID. 

We'll need the express-session, so install it using:
```
npm install --save express-session
```

The following code sets a session and everytime a new visit is made it updates the count of visit and displays that.
```
var express = require('express');
var cookieParser = require('cookie-parser');
var session = require('express-session');

var app = express();

app.use(cookieParser());
app.use(session({secret: "Shh, its a secret!"}));

app.get('/', function(req, res){
    if(req.session.page_views){
        req.session.page_views++;
        res.send("You visited this page " + req.session.page_views + " times");
    }
    req.session.page_views = 1;
    res.send("Welcome to this page for the first time!");
});

app.listen(3000);
```
Generally if you dont have multiple sites then it we can use sessions otherwise we need to expire sessions from all the servers. When logging in create a cookie and a session corresponding to the cookie. If the session is present then the user is signedin. When logging out delete the session and then delete the cookie (invalidate)

How to use sessions for authentication?
Sessions can be used to store user info. Use cookie to validate and create session. When logging out delete the session, invalidate the cookie and delete the cookie.
Where to set the session expiry?

# AUTHENTICATION
# =======================================================
How to use cookies along with sessions to authenticate users.
Authentication is a process in which the credentials provided are compared to those on file in a database of authorized users' information on a local operating system or within an authentication server. If the credentials match, the process is completed and the user is granted authorization for access.
For us to create an authentication system, we will need to create a sign up page and a user-password store. The following code creates an account for us and stores it in memory. This is just for demo purposes, ALWAYS use a persistent storage(database or files) to store user information. 

```
var express = require('express');
var app = express();
var bodyParser = require('body-parser');
var multer = require('multer');
var upload = multer(); 
var session = require('express-session');
var cookieParser = require('cookie-parser');

app.set('view engine', 'pug');
app.set('views','./views');

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true })); 
app.use(upload.array());
app.use(cookieParser());
app.use(session({secret: "Your secret key"}));

var Users = [];

app.get('/signup', function(req, res){
    res.render('signup');
});

app.post('/signup', function(req, res){
    if(!req.body.id || !req.body.password){
        res.status("400");
        res.send("Invalid details!");
    }
    else{
        Users.filter(function(user){
            if(user.id === req.body.id){
                res.render('signup', {message: "User Already Exists! Login or choose another user id"});
            }
        });
        var newUser = {id: req.body.id, password: req.body.password};
        Users.push(newUser);
        req.session.user = newUser;
        res.redirect('/protected_page');
    }
});

function checkSignIn(req, res, ){
    if(req.session.user){
        next();     //If session exists, proceed to page
    } else {
        var err = new Error("Not logged in!");
    console.log(req.session.user);
        next(err);  //Error, trying to access unauthorized page!
    }
}

app.get('/protected_page', checkSignIn, function(req, res){
    res.render('protected_page', {id: req.session.user.id})
});

app.get('/login', function(req, res){
    res.render('login');
});

app.post('/login', function(req, res){
    console.log(Users);
    if(!req.body.id || !req.body.password){
        res.render('login', {message: "Please enter both id and password"});
    }
    else{
        Users.filter(function(user){
            if(user.id === req.body.id && user.password === req.body.password){
                req.session.user = user;
                res.redirect('/protected_page');
            }
        });
        res.render('login', {message: "Invalid credentials!"});
    }
});

app.get('/logout', function(req, res){
    req.session.destroy(function(){
        console.log("user logged out.")
    });
    res.redirect('/login');
});

app.use('/protected_page', function(err, req, res, next){
console.log(err);
    //User should be authenticated! Redirect him to log in.
    res.redirect('/login');
});

app.listen(3000);
```



# RESTFUL APIS
# =======================================================
A popular architectural style of how to structure and name these APIs and the endpoints is called REST(Representational Transfer State). HTTP 1.1 was designed keeping REST principles in mind. REST was introduced by Roy Fielding in 2000 in his paper Fielding Dissertions. 


**index.js**
```
var express = require('express');
var bodyParser = require('body-parser');
var multer = require('multer');
var upload = multer();

var app = express();

app.use(cookieParser());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(upload.array());

//Require the Router we defined in movies.js
var movies = require('./movies.js');

//Use the Router on the sub route /movies
app.use('/movies', movies);

app.listen(3000);
```

We are currently storing the movies as variable in memory.
**movies.js**
```
var express = require('express');
var router = express.Router();
var movies = [
    {id: 101, name: "Fight Club", year: 1999, rating: 8.1},
    {id: 102, name: "Inception", year: 2010, rating: 8.7},
    {id: 103, name: "The Dark Knight", year: 2008, rating: 9},
    {id: 104, name: "12 Angry Men", year: 1957, rating: 8.9}
];


// The get Route
router.get('/:id([0-9]{3,})', function(req, res){
    var currMovie = movies.filter(function(movie){
        if(movie.id == req.params.id){
            return true;
        }
    });
    if(currMovie.length == 1){
        res.json(currMovie[0])
    }
    else{
	    //Set status to 404 as movie was not found
        res.status(404);
        res.json({message: "Not Found"});
    }
});


// The Post Route
router.post('/', function(req, res){
    //Check if all fields are provided and are valid:
    if(!req.body.name || 
        !req.body.year.toString().match(/^[0-9]{4}$/g) || 
        !req.body.rating.toString().match(/^[0-9]\.[0-9]$/g)){
        res.status(400);
        res.json({message: "Bad Request"});
    }
    else{
        var newId = movies[movies.length-1].id+1;
        movies.push({
            id: newId,
            name: req.body.name,
            year: req.body.year,
            rating: req.body.rating
        });
        res.json({message: "New movie created.", location: "/movies/" + newId});
    }
});


// The Put Route
router.put('/:id', function(req, res){
    //Check if all fields are provided and are valid:
    if(!req.body.name || 
        !req.body.year.toString().match(/^[0-9]{4}$/g) || 
        !req.body.rating.toString().match(/^[0-9]\.[0-9]$/g) ||
        !req.params.id.toString().match(/^[0-9]{3,}$/g)){
        res.status(400);
        res.json({message: "Bad Request"});
    }
    else{
        //Gets us the index of movie with given id.
        var updateIndex = movies.map(function(movie){
            return movie.id;
        }).indexOf(parseInt(req.params.id));
        if(updateIndex === -1){

            //Movie not found, create new
            movies.push({
                id: req.params.id,
                name: req.body.name,
                year: req.body.year,
                rating: req.body.rating
            });
            res.json({message: "New movie created.", location: "/movies/" + req.params.id});    
        
        }else{
            //Update existing movie
            movies[updateIndex] = {
                id: req.params.id,
                name: req.body.name,
                year: req.body.year,
                rating: req.body.rating
            };
            res.json({message: "Movie id " + req.params.id + " updated.", location: "/movies/" + req.params.id});
        }
    }
});


// The delete route
router.delete('/:id', function(req, res){
    var removeIndex = movies.map(function(movie){
        return movie.id;
        //Gets us the index of movie with given id.
    }).indexOf(req.params.id); 
    if(removeIndex === -1){
        res.json({message: "Not found"});
    }else{
        movies.splice(removeIndex, 1);
        res.send({message: "Movie id " + req.params.id + " removed."});
    }
});

module.exports = router;
```


# STORING IN FILES
# =======================================================
Node FS module for reading from files.

# DEBUGGING
# =======================================================
Express uses the Debug module to internally log information about route matching, middleware functions, application mode, etc.
```
DEBUG=express:* node index.js
```
These logs are very helpful when a component of your app is not functioning right. This verbose output might be a little overwhelming. You can also restrict the DEBUG variable to specific area to be logged. For example, if you wish to restrict the logger to application and router, you can use:

```
DEBUG=express:application,express:router node index.js
```

Debug is turned off by default and is automatically turned off in production environment. Debug can also be extended to meet your needs, you can read more about it at its npm page.

# PERFORMANCE TESTING
# =======================================================

SERVER
Nodemon
build

DEBUG=myapp:* npm start
set DEBUG=myapp:* & npm start

# PRODUCTION INSTALLATION
# =======================================================
Memory leaks
https://www.npmjs.com/package/memwatch
# UNIT TESTS


# WEBSITES
https://strongloop.com/strongblog/category/express
http://www.hacksparrow.com/category/express-js
https://codeforgeek.com/2014/10/express-complete-tutorial-part-1/
http://node-tricks.com/category/express/
https://www.rosehosting.com/blog/tag/express/

https://expressjs.com/en/resources/books-blogs.html