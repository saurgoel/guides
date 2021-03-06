http://nodetuts.com/
http://nodepatternsbooks.com/
https://anotheruiguy.gitbooks.io/nodeexpreslibsass_from-scratch/content/node-npm.html

Node is based on the Javascript V8 engine that is what browsers use.
Debugging Javascript

require in JavaScript
promises in JavaScript



    npm install will install both "dependencies" and "devDependencies"

    npm install --production will only install "dependencies"

    npm install --dev will only install "devDependencies"



###### Commiting Node modules
If you wish to lock down the specific bytes included in a package, for example to have 100% confidence in being able to reproduce a deployment or build, then you ought to check your dependencies into source control, or pursue some other mechanism that can verify contents rather than versions.
Shannon and Steven mentioned this before but I think, it should be part of the accepted answer.

Update
The source listed for the below recommendation has been updated. They are no longer recommending the node_modules folder be committed.
Usually, no. Allow npm to resolve dependencies for your packages.
For packages you deploy, such as websites and apps, you should use npm shrinkwrap to lock down your full dependency tree:


# BASICS
# =======================================================
NodeJS is an event-driven and non-blocking I/O javascript platform.
In case of unicorn it has a master process which spawn several child processes
In case of node there are workers
Node.js has more than proven itself capable of handling
multiple events concurrently such as server connections, and all without
exposing us to the complexities of threading. Still, this locks
our apps down to a single process with a single thread of execution
consuming a single event queue. On a machine with a single processor, this
is no big loss; there is only one active process in any case.
how node js can be very good
considering node js to be a new web framework it may not integrate well with the native systems
like rails does (like databases etc.) and it may not have so much MVC inbuilt
but it is a good replacement to the back end requirements of front end (to replace website controller for example)
This makes the front end more consistent (javscript only) and the web becomes just like another platform (loke mobile)
where the entire structure front end + back end is built upon a single language.
node js is good at maintaining connections from the client side and can interact using http apis with the back end
NODE JS



# ISOMORPHIC JAVASCRIPT
One of the advantages of having Node.js as runtime for the backend of a web application is that we have to deal only with JavaScript as a single language across the web stack. With this capability it is totally legit to be willing to share some code between the frontend and the backend to reduce the code duplication between the browser and the server to the bare minimun. 
The art of creating JavaScript code that is "environment agnostic" is today being recognized as "Universal JavaScript", term that — after a very long debate — seems to have won a war against the original name "Isomorphic JavaScript".

# ADVANTAGES
I think the advantages are:
Web development in a dynamic language (JavaScript) on a VM that is incredibly fast (V8). It is much faster than Ruby, Python, or Perl.
Ability to handle thousands of concurrent connections with minimal overhead on a single process.
JavaScript is perfect for event loops with first class function objects and closures. People already know how to use it this way having used it in the browser to respond to user initiated events.
A lot of people already know JavaScript, even people who do not claim to be programmers. It is arguably the most popular programming language.
Using JavaScript on a web server as well as the browser reduces the impedance mismatch between the two programming environments which can communicate data structures via JSON that work the same on both sides of the equation. Duplicate form validation code can be shared between server and client, etc.


# A single node process can run per core
Node Cluster Module
There is a master process that can start child processes
Each process runs on a core
Number CPU cores = no. of processes

# Load balancing
Load balancing can be done using a reverse proxy server

# Installing node and express on centos
# https://www.vultr.com/docs/installing-nodejs-and-express-on-centos




# NODE PACKAGE MANAGER
# =======================================================
###### Node package manager
install packages globally
```
npm install --global gulp
```

packages liek grunt and gulp can be installed globallby
but packages that are requied by the app and that have depenencies must be installed locally

###### Node Packages
A package is:
a) a folder containing a program described by a package.json file
b) a gzipped tarball containing (a)
c) a url that resolves to (b)
d) a <name>@<version> that is published on the registry (see npm-registry) with (c)
e) a <name>@<tag> that points to (d)
f) a <name> that has a "latest" tag satisfying (e)
g) a <git remote url> that resolves to (a)


# package.json
# =====================================
Most people are aware that is is possible to define scripts in package.json which can be run with npm start or npm test, but npm scripts can do a lot more than simply start servers and run tests.
start, actually defaults to node server.js, so the above declaration is redundant. In order for the test command to work with mocha, I also need to include it in the devDependencies section (it works in the dependencies section also, but since it is not needed in production it is better to declare it here).

The reason the above test command, mocha --reporter spec test, works is because npm looks for binaries inside node_modules/.bin and when mocha was installed it installed mocha into this directory.

The code that describes what will be installed into the bin directory is defined in mocha's package.json and it looks like this:

// Macha package.json
{
  "name": "mocha",
  ...
  "bin": {
    "mocha": "./bin/mocha",
    "_mocha": "./bin/_mocha"
  },
  ...
}
As we can see in the above declaration, mocha has two binaries, mocha and _mocha.

Many packages have a bin section, declaring scripts that can be called from npm similar to mocha. To find out what binaries we have in our project we can run ls node_modules/.bin

# Scripts availble in one of my projects
$ ls node_modules/.bin
_mocha      browserify  envify      jshint
jsx         lessc       lesswatcher mocha
nodemon     uglifyjs    watchify


###### Concurrent
Sometimes it is also nice to be able to run multiple commands at the concurrently. This is easily done by using &amp; to run them as background jobs.

```
  "scripts": {
    // Run watch-js, watch-less and watch-server concurrently
    "watch": "npm run watch-js & npm run watch-less & npm run watch-server",
    "watch-js": "watchify app/js/main.js -t reactify -o static/bundle.js -dv",
    "watch-less": "nodemon --watch app/less/*.less --ext less --exec 'npm run build-less'",
    "watch-server": "nodemon --ignore app --ignore static server.js"
  },
  // Add required dependencies
  "devDependencies": {
    "watchify": "^0.6.2",
    "nodemon": "^1.0.15"
  }
```
The above scripts contain a few interesting things. First of all watch uses &amp; to run three watch jobs concurrently. When the command is killed, by pressing Ctrl-C, all the jobs are killed, since they are all run with the same parent process.

watchify is a way to run browserify in watch mode. watch-server uses nodemon in the standard way and restarts the server whenever a relevant file has changed.



# Node Modules
# =====================================
By default, locally-installed packages go into ./node_modules. global ones go into the prefix config variable (/usr/local by default).
You can run npm config list to see your current config and npm config edit to change it

###### NPM INSTALL
Install the dependencies in the local node_modules folder.
By default npm install, installs packages in the production environment
devDependeies are threfore not installed (for it dev enviroment has to be passed)
Packages are installed locally and/or globally
Locally they are installed in the local node_modules directory

###### using -g option
npm install -g some_package

the node app you are building is itself a package
only a single package can be installed globally
a -> x, y, z
then a will be installed globally
x, y, z will be bundled inside it

###### Install a package that is sitting in a folder on the filesystem.
```
npm install <folder>
```

###### Install tarballed package
```
npm install <tarball file>
npm install <tarball url>
```

###### This installs the node modules to the directory that is specificed in prefix
npm get prefix

How npm searches for packages
npm looks for packages in the current_directory node_modules directory
Then in all the parent directories that have the folder node_modules
Then finally in the global node_modules direcory

There may be permission issues that appear
These can be fixed by fixing the permissions of these folder

Either chown these directories. But it is not recommended as these may be used by some other user as well.
```
sudo chown -R $USER /usr/local/lib/node_modules
sudo chown -R $USER /usr/local/bin/npm
sudo chown -R $USER /usr/local/bin/node
```

###### Changing the default node_modules directories
```
mkdir ~/npm-global
npm config set prefix '~/npm-global'
export PATH=~/npm-global/bin:$PATH
source ~/.profile
```

###### Now the package with -g will install in that directory
```
npm install -g jshint
```

###### Node Modules directory
When package.json is present then node modules are searched for to run a basic node app with all the npm packages install the node packages it looks for package.json for all the files
npm install

There are some cases when some of the packages are not installed like express
npm install express

###### Aditional options for npm install
```
--save : Package will appear in your dependencies
--save-dev :
--save-optional :
```


# INSTALLING NODE JS
# =======================================================
NPM is like bundler. That manages the installation of all the libraries for node js (just like ruby libraries). NodeJS is like rails written in JS. NPM has been named incorrectly. It is a general JS package manager. When you install node then npm also gets installed along with it.

###### INSTALLING on Mac using DMG
download from here https://nodejs.org/en/download/


###### INSTALLING from Binaries
Installing node from source (binaries)
```
yum -y install gcc gcc-c++ make
wget https://nodejs.org/dist/v0.12.7/node-v0.12.7.tar.gz
tar xzf node-v0.12.7.tar.gz
cd node-v0.12.7
./configure
make
make install
```

###### Installing node using nvm (node version manager)
Nvm is like rvm. Its a version manager for node and allows easy switching.
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash
nvm ls-remote
nvm install stable
```
Using different nvm version
```
nvm use 0.12.22
nvm list
nvm install stable
nvm ls-remote
```

###### INSTALLING NODE AND NPM CENTOS
install npm can be installed from yum repo. Now generally npm gets installed along with node js
```
yum -y install nodejs npm
npm --version
```


# BASICS
# =======================================================
###### Version of Node
node --version

###### Basic Server
Starting a basic server. add a server config file. replace the APP_PRIVATE with the 127.0.0.1
```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(8080, 'APP_PRIVATE_IP_ADDRESS');
console.log('Server running at http://APP_PRIVATE_IP_ADDRESS:8080/');
```
to start the server (this is not a daemon)
```
node hello.js
```

# RUNNING NODE IN BACKGROUND
# =======================================================
###### PM2 PROCESS MANAGER
to run a daemon process. Now we will install PM2, which is a process manager for Node.js applications. PM2 provides an easy way to manage and daemonize applications (run them as a service).
-g means the global option
```
sudo npm install pm2@latest -g
```

pm2 can start and run applications in the background
```
pm2 start hello.js
```

###### Viewing the pm2 process list
pm2 startup command (need to specify the platform too currently centos)
Applications that are running under PM2 will be restarted automatically if the application crashes or is killed, but an additional step needs to be taken to get the application to launch on system startup
```
pm2 startup centos
```

###### This command has to be run as root
```
sudo su -c "env PATH=$PATH:/usr/local/bin pm2 startup centos -u deployer --hp /home/deployer"
```

###### Stoping and starting specific dameons
```
pm2 stop example
pm2 restart example
pm2 list
pm2 info example
pm2 monit

pm2 stop all
pm2 delete all
```

###### Viewing node server logs
```
pm2 logs
```
1. Do not run app as root as it is more secure
2. Your application will restart if it crashes, and it will keep a log of unhandled exceptions.
3. Your application will restart when the server starts - i.e. it will run as a service.
4. PM2 will keep a log of your unhandled exceptions - in this case, in a file at /home/safeuser/.pm2/logs/app-err.log.

###### gulp build
```
gulp build
```

When the gulp build is complete then the server can be started using the following command
```
node build/server.js
pm2 start build/server.js
```

run the app as a services
Run this command to run your application as a service by typing the following:
Replace the safeuser with the custom username
sudo env PATH=$PATH:/usr/local/bin pm2 startup -u safeuser

Specifying the port
```
PORT=8008 node server.js
PORT=3002 pm2 start -I 0 ./bin/www
```


# Using Supervisord
Using supervisord instead of pm2
```
supervisor ./bin/www
```

# Using Tmuxinator
We can also start a screen/tmux session and leave it open
```
tmuxinator start amber
```

###### Automated deployment of nodejs without restarts etc.
The challenges are many-fold – packaging and dependency management, single step deploy, and start/re-start without bringing down the house!
If you don’t shrinkwrap, each run of npm install during deployment may fetch different versions of the dependencies. You can shrinkwrap, but even when shrinkwrapped, npm install fetches those dependencies live at deploy time. If your npm server, or npmjs.org is down, you won’t be able to deploy. This is not acceptable.
Javascript doesn’t require compilation, many applications still require building.

# RUNNING MULTIPLE NODE INSTANCES
# =======================================================
###### Node Cluster Module
```
cluster = require 'cluster'
numCPUs = require('os').cpus().length

exports = module.exports = launch: ->
  console.log 'Before the fork'

  if (cluster.isMaster)
    console.log 'I am the master, launching workers!'
    cluster.fork() for i in [0...numCPUs]
  else
    console.log 'I am a worker!'

  console.log 'After the fork'
view raw
```

###### Cluster Module
While the exec and spawn methods allow calling external commands, of interest to us is again the fork

###### Starting node nodes
These child Nodes are still whole new instances of V8. Assume at least
30ms startup and 10mb memory for each new Node. That is, you cannot
create many thousands of them.

###### In summary
Use either the child_process or the cluster modules to take advantage of multi-processer environments.
Use cluster when you want to parallelize the SAME flow of execution and server listening.
Use child_process when you want DIFFERENT flows of execution working together.
Take advantage of built in Inter-Process Communication to pass objects between the processes.

The elegant solution Node.js provides for scaling up the applications is to split a single process into multiple processes or workers, 
in Node.js terminology. This can be achieved through a cluster module. The cluster module allows you to create child processes (workers),
which share all the server ports with the main Node process (master).

Node.js documentation suggests using the default round-robin style as the scheduling policy.

```
var cluster = require('cluster');
var http = require('http');
var numCPUs = 4;

if (cluster.isMaster) {
    for (var i = 0; i < numCPUs; i++) {
        cluster.fork();
    }
} else {
    http.createServer(function(req, res) {
        res.writeHead(200);
        res.end('process ' + process.pid + ' says hello!');
    }).listen(8000);
}
```

###### Cluster module with the express module
```
var cluster = require('cluster');

if(cluster.isMaster) {
    var numWorkers = require('os').cpus().length;

    console.log('Master cluster setting up ' + numWorkers + ' workers...');

    for(var i = 0; i < numWorkers; i++) {
        cluster.fork();
    }

    cluster.on('online', function(worker) {
        console.log('Worker ' + worker.process.pid + ' is online');
    });

    cluster.on('exit', function(worker, code, signal) {
        console.log('Worker ' + worker.process.pid + ' died with code: ' + code + ', and signal: ' + signal);
        console.log('Starting a new worker');
        cluster.fork();
    });
} else {
    var app = require('express')();
    app.all('/*', function(req, res) {res.send('process ' + process.pid + ' says hello!').end();})

    var server = app.listen(8000, function() {
        console.log('Process ' + process.pid + ' is listening to all incoming requests');
    });
}
```

http://www.sitepoint.com/how-to-create-a-node-js-cluster-for-speeding-up-your-apps/

As you would probably know, Node.js is a platform built on Chrome's JavaScript runtime, gracefully named V8.
The V8 engine, and hence Node.js, runs in a single-threaded way, therefore, doesn't take advantage of multi-core systems capabilities.

Luckily enough, Node.js offers the cluster module, which basically will spawn some workers which can all share any TCP connection.

When requests are received, they are distributed one at a time to each worker. If a worker is available, it immediately starts processing the request; otherwise it’ll be added to a queue.
Like rails unicorn does not handle this. Cluster module is internally built into node to create workers


# PM2 Cluster module
# There are 2 modes in pm2
- Cluster Mode 
- Fork Mode (default)
# The mode has to be specified before the link is created. Otherwise it has to be deleted and created again

# Zero downtime deployments using the cluster module
npm install --save source-map-support


# No of processes in centos
nproc

# all info about the cpu
lscpu

# Pm2 to the resuce
# pm2 handles the above logic of starting the cluster and managing the workers
# use the below code to start the workers
 pm2 start app.js -i 4
# Cores equal to the number of cores. If 'number of workers' argument is 0, PM2 will automatically spawn as many workers as you have CPU cores.

###### Restarting Stale workers
If any of your workers happens to die, PM2 will restart them immediatly so you don't have to worry about that either.

###### Scaling the app (running)
It can also be an addition such as pm2 scale app +3 in which case 3 more workers will be added to the cluster.
```
pm2 scale app 2
pm2 scale app +2
```

###### Updating apps with zero downtime (changes). Just like unicorn reload
PM2 reload <app name> feature will restart your workers one by one, and for each worker, wait till the new one has spawned before killing the old one.
This way, your server keeps running even when you are deploying the new patch straight to production.
```
pm2 reload appName
```

###### Graceful reloads
You can also use gracefulReload feature which does pretty much the same thing but instead of immediatly killing the worker it will send it a shutdown signal via IPC so it can close ongoing connections or perform some custom tasks before exiting
pm2 gracefulReload appName

###### View Current workers (child processes)
pm2 list appName

###### Using pm2 deploy
https://keymetrics.io/2014/06/25/ecosystem-json-deploy-and-iterate-faster/
there is an ecosystem file that is requried

# LOGGING
# =======================================================

Morgan is another HTTP request logger middleware for Node.js. It simplifies the process of logging requests to your application.  You might think of Morgan as a helper that collects logs from your server, such as your request logs. It saves developers time because they don’t have to manually create common logs. It standardizes and automatically creates request logs.
Morgan can operate standalone, but commonly it’s used in combination with Winston. Winston is able to transport logs to an external location, or query them when analyzing a problem.

# TASK RUNNERS - GULP
# =======================================================
Gulp - you can specify tasks in the gulp
Gulp will perform the tasks for you. It is a build system (task performer).
Gulp streamlines the build flow. For example compiling files and wrinting them.
Using power of node streams, gulp builds are fast
gulpfile.js is created at the root of the project
```
var gulp = require('gulp');

gulp.task('default', function() {
  // place code for your default task here
});
```
run gulp
```
gulp
```

http://stackoverflow.com/questions/33561272/task-runners-gulp-grunt-etc-and-bundlers-webpack-browserify-why-use-toge
Grunt and Gulp are actually task runners, and they have differences like config driven tasks versus stream based transformations. Each has its own strengths and weaknesses, but at the end of the day, they pretty much help you create tasks that can be run to solve a larger build problem. Most of the time, they have nothing to do with the actual run-time of the app, but rather they transform or they put files, configs and other things in place so that the run time works as expected. Sometimes they even spawn servers or other processes that you need to run your app.

Webpack and Browserify are package bundlers. Basically, they are designed to run through all of a package's dependencies and concatenate their source into one file that (ideally) can be used in a browser. They are important to modern web development, because we use so many libraries that are designed to run with Node.js and the v8 compiler. Again, there are pros and cons and different reasons some developers prefer one or the other (or sometimes both!). Usually the output bundles of these solutions contain some sort of bootstrapping mechanisms to help you get to the right file or module in a potentially huge bundle.

The blurred line between runners and bundlers might be that bundlers can also do complex transformations or trans-pilations during their run-time, so they can do several things that task runners can do. In fact, between browserify and webpack there's probably around a hundred transformers that you can use to modify your source code. For comparison, there's at least 2000 gulp plugins listed on npm right now. So you can see that there are clear (hopefully... ;)) definitions of what works best for your application.

That being said, you might see a complex project actually using both task-runners and package bundlers at the same time or in tandem. For example at my office, we use gulp to start our project, and webpack is actually run from a specific gulp task that creates the source bundles that we need to run our app in the browser. And because our app is isomorphic, we also bundle some of the server code.

# PM2
# =======================================================

# Using Screen
# =======================================================
This might not be the accepted way, but I do it with screen, especially while in development because I can bring it back up and fool with it if necessary.

screen
node myserver.js
> > CTRL-A then hit D
> > The screen will detach and survive you logging off. Then you can get it back back doing screen -r. Hit up the screen manual for more details. You can name the screens and whatnot if you like.

# FOREVER
# =======================================================
npm install forever --save-dev
./node_modules/bin/forever 

forever start app.js

forever --help

JSON Configuration
// forever/development.json
{
    // Comments are supported
    "uid": "app",
    "append": true,
    "watch": true,
    "script": "index.js",
    "sourceDir": "/home/myuser/app"
}


forever start ./forever/development.json
forever start /home/myuser/app/forever/development.json
./node_modules/.bin/forever stop build/server.js

forever stopall
forever restart
forever restartall

start               Start SCRIPT as a daemon
stop                Stop the daemon SCRIPT by Id|Uid|Pid|Index|Script
stopall             Stop all running forever scripts
restart             Restart the daemon SCRIPT
restartall          Restart all running forever scripts
list                List all running forever scripts
config              Lists all forever user configuration
set <key> <val>     Sets the specified forever config <key>
clear <key>         Clears the specified forever config <key>
logs                Lists log files for all forever processes
logs <script|index> Tails the logs for <script|index>
columns add <col>   Adds the specified column to the output in `forever list`
columns rm <col>    Removed the specified column from the output in `forever list`
columns set <cols>  Set all columns for the output in `forever list`
cleanlogs           [CAREFUL] Deletes all historical forever log files

**forever.start (file, options)**
Starts a script with forever. The options object is what is expected by the Monitor of forever-monitor.

**forever.startDaemon (file, options)**
Starts a script with forever as a daemon. WARNING: Will daemonize the current process. The options object is what is expected by the Monitor of forever-monitor.

**forever.stop (index)**
Stops the forever daemon script at the specified index. These indices are the same as those returned by forever.list(). This method returns an EventEmitter that raises the 'stop' event when complete.

**forever.stopAll (format)**
Stops all forever scripts currently running. This method returns an EventEmitter that raises the 'stopAll' event when complete.

The format parameter is a boolean value indicating whether the returned values should be formatted according to the configured columns which can set with forever columns or programmatically forever.config.set('columns').

**forever.list (format, callback)**
Returns a list of metadata objects about each process that is being run using forever. This method will return the list of metadata as such. Only processes which have invoked forever.startServer() will be available from forever.list()

The format parameter is a boolean value indicating whether the returned values should be formatted according to the configured columns which can set with forever columns or programmatically forever.config.set('columns').

**forever.tail (target, options, callback)**
Responds with the logs from the target script(s) from tail. There are two options:

length (numeric): is is used as the -n parameter to tail.
stream (boolean): is is used as the -f parameter to tail.
**forever.cleanUp ()**
Cleans up any extraneous forever *.pid files that are on the target system. This method returns an EventEmitter that raises the 'cleanUp' event when complete.

**forever.cleanLogsSync (processes)**
Removes all log files from the root forever directory that do not belong to current running forever processes. Processes are the value returned from Monitor.data in forever-monitor.

forever.startServer (monitor0, monitor1, ..., monitorN)
Starts the forever HTTP server for communication with the forever CLI. NOTE: This will change your process.title. This method takes multiple forever.Monitor instances which are defined in the forever-monitor dependency.


# TASK RUNNERS - GRUNT
# =======================================================
So, when Gulp came out, I was right at the point of willing to dive deep into JS-based task runners. 




# NODEJS BASED FRAMEWORKS
# =======================================================
Sails.js is the classic (but updated) MVC framework, so for complex applications you have a good structure to start with. The development community is active, they are getting plugins sorted out, and the platform is database and storage and front end agnostic.
Meteor.js has a LOT of momentum at the moment, and a very active middleware and plugin community.  For full functionality mongo.db is the database, but that includes a lot of magic freebies such as publish and subscribe on both client and server, automatic updates as data changes, and flexibility on storing less structured information.  
Meteor is client render only, and is not brilliant for seo as suggested, but then again neither is sails (stop press, Meteor does now have a server side renderer for SEO).  With these platforms I would consider static heavily optimized pages for seo or something like WordPress with a really good cache plugin plus there are a lot of people using phantom.js to render and cache pages for the robots.(plus CDN for all options!) 
Express used to be the defacto way get your node server on the web quickly platform, and is very unopinionated.  There are so many other platforms layered on top, like the MEAN stack (and Sails used Express and is still Express compatible). So even though you cant compare Express directly with Meteor or Sails you can roll your own stack with full choice of templating, database, layout, websockets, and rest frameworks/plugins.



# Express and Geddy are the main contenders at the moment, and I think either are comparable to Django and CodeIgniter.
I have personally gone with Express as my framework of choice. Some of this is simply a matter of preference—Express is more from the Sinatra school of thought, Geddy seems more attuned to Rails, if you know what I mean. I also think the development around Express is a little more active.
Update [2011-06-21] On a current project that is largely REST API-based, I am working with the Connect middleware directly and finding it most excellent. Connect and Express are still my pick for node framework of champions.



# BENCHMARKING (using seiege)
# =======================================================
would start hitting the servers
yum install siege

# c 100 means 100 consecutive users
siege -c100 -t1M http://localhost:3000/

Mongoose for MongoDB
Sequelize for Postgre

# TESTING
Unit Testing with webpack, karma, jsdom, mocha, sinon & enzyme

    Reducers
    Components (Enzyme)
    Synchronous and Asynchronous Actions