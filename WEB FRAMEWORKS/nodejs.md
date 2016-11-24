http://nodetuts.com/
http://nodepatternsbooks.com/
https://anotheruiguy.gitbooks.io/nodeexpreslibsass_from-scratch/content/node-npm.html

require in JavaScript
promises in JavaScript

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


###### NODE MODULES
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


###### Using supervisord instead of pm2
```
supervisor ./bin/www
```

###### We can also start a screen/tmux session and leave it open
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