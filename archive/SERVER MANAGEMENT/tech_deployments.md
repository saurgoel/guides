setting up vms
managing vms
openssh
tunnelblick
server to server call
client to server calls
I’m going to explain my setup which uses subdomains for a virtual machine, and apache’s vhost_alias module with the VirtualDocumentRoot directive.

# in case of deployments

# web servers and their roles
Phusion Passenger Application Server
Nginx HTTP Server Running As Reverse-Proxy

# getting the environment setup
Updating And Preparing The Operating System
Setting Up Ruby Environment and Rails
Downloading And Installing The Server Applications

# preparing application for deployment
Creating A Sample Application / Uploading Your Source Code
Creating The Nginx Management Script
Configuring Nginx


Here the applications are modern that are Ruby Rack, Python WSGI  (having these CGIs)
CGI is the interface between the server and the application
Nginx, on the other hand, is designed from ground up to act as a multi-purpose HTTP server. It is capable of serving static files (e.g. images, text files etc.) extremely well, balance connections, and deal with certain exploits attempts.
It acts as the first entry point of all requests, and passes them to Passenger for the web application to process and return a response.


# Passengre application server
Passenger today has become the recommended server for Ruby on Rails applications. It is a mature, feature rich product which aims to cover necessary needs and areas of application deployment whilst greatly simplifying the set-up and getting-started procedures. It eliminates the traditional middleman server set up architecture by direct integration with Nginx (and Apache as well). It is also referred to as mod_rails.
The open-source version, which we will be using, has a multi-process single-threaded operation mode.
Its Enterprise version can be configured to work either single-threaded or multi-threaded.

# Nginx server
Nginx is a very high performant web server / (reverse)-proxy. It has reached its popularity due to being light weight, relatively easy to work with, and easy to extend (with add-ons / plug-ins). Thanks to its architecture, it is capable of handling a lot of requests (virtually unlimited), which - depending on your application or website load - could be really hard to tackle using some other older alternatives.
Remember: "Handling" connections technically means not dropping them and being able to serve them with something. You still need your application and database functioning well in order to have Nginx serve clients responses that are not error messages.


# Capistrano
Capistrano: a Ruby based remote server automation tool which can be easily used to automate mundane deployment and system management tasks. Using Capistrano, you can almost entirely automate all actions you would normally take to get your product live.
Capistrano, as mentioned in our introduction, is a Ruby based, open-source server management tool.
helping you with almost anything from getting the code on your deployment machine to bootstrapping the deployment process -- and it can do this on multiple systems at once or in a round-robin fashion.

# Capistrano
Ruby Programming Language
Capistrano Recipes
System/Server Administration
Application Deployment

# Installing Capistrano
Preparing The System
Installing Ruby
Installing Capistrano

# Getting Started With Capistrano
Capistrano Basics
Initiating Capistrano Inside A Project
Creating Users For Deploying With Capistrano

# system administragion jobs performed by capistrano
System and server administration jobs (usually) include pretty much everything that relates to:
Building a server
Installing applications
Maintenance of systems running these applications
And monitoring







# =========================== CAPISTRANO basics
capistrano contains deloy.rb that is same across all the different sstage
different stages include, production.rb, staging.rb etc.

#  ============================ INSTALLING CAPISTRANO
# Capistrano Recipes
Recipes in Capistrano lingo translate to files which contain operative directions for deploying (or managing) applications and servers.

# App deployment
app deployment requires running some commands in a specified order
these needs to be documented

# installing capistrano
yum -y update
# install development tools (install development tools bundle centos)
yum groupinstall -y 'development tools'

# install rvm
curl -L get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
# install rvm
rvm reload
rvm install 2.1.0

# installing capistrnaon
gem install capistrano
cap --version


# No need to include capistrano in gem file
cd my_app
cap install
# good eay of executing recipes is to create a user other than root. We create a deployers group and give them permisson to proceed.
groupadd deployers
# Usage: sudo usermod -a -G deployers [name]
sudo usermod -a -G deployers deployer

# give deployers sudo permission
vi /etc/sudoers

## Allows people in group wheel to run all commands
%deployers      ALL=(ALL) ALL


# ================================ INSTALLING PASSENGER AND NGINX
# prepareing deployment server
Updating And Preparing The Operating System
Setting Up Ruby Environment and Rails
Downloading And Installing App. & HTTP Servers
Creating The Nginx Management Script
Configuring Nginx For Application Deployment
Downloading And Installing Capistrano
Creating A System User For Deployment

# setting up capistrano for app
Installing Capistrano Inside The Project Directory
Working With config/deploy.rb Inside The Project Directory
Working With config/deploy/production.rb Inside The Project Directory
Deploying To The Production Server

# If your VPS has less than 1 GB of RAM, you will need to perform the below simple procedure to prepare a SWAP disk space to be used as a temporary data holder (RAM substitute).
# Create a 1024 MB SWAP space
sudo dd if=/dev/zero of=/swap bs=1M count=1024
sudo mkswap /swap
sudo swapon /swap

# install passenger
gem install passenger

# setting up nginx with passegner can be done via the passenger gem itself
# Run the following to start compiling Nginx with native Passenger module:
passenger-install-nginx-module

# Creating NGINX management script
# Run the following commands to create the script:
vi /etc/rc.d/init.d/nginx

# set mode of maangemetn of script as executable
chmod +x /etc/rc.d/init.d/nginx


# adding nginx config script (configuration of passenger contained)
# open up this configuration file to edit
vi /opt/nginx/conf/nginx.conf


As the first step, find the http { node and append the following right after the passenger_root and passenger_ruby directives:

# Only for development purposes.
# Remove this line when you upload an actual application.
# For * TESTING * purposes only.
passenger_app_env development;

Scroll down the file and find server { ... Comment out the default location, i.e.:

..

#    location / {
#            root   html;
#            index  index.html index.htm;
#        }

..


And define your default application root:

# Set the folder where you will be deploying your application.
# We are using: /home/deployer/apps/my_app
root              /home/deployer/apps/my_app/public;
passenger_enabled on;


# Run the following to reload the Nginx with the new application configuration:
/etc/init.d/nginx restart
# To check the status of Nginx, you can use:
/etc/init.d/nginx status


# ================================ FINAL CAPISTRANO INTEGRATIO
# go inside the project directory and fo
cap install

# mkdir -p config/deploy
# create config/deploy.rb
# create config/deploy/staging.rb
# create config/deploy/production.rb
# mkdir -p lib/capistrano/tasks
# Capified







# ========================= USING UNICORN as the APP SERVER
Unicorn lets me do zero-downtime deployments with it’s rolling restart functionality

https://www.digitalocean.com/community/tutorials/how-to-deploy-rails-apps-using-unicorn-and-nginx-on-centos-6-5
https://www.digitalocean.com/community/tutorials/how-to-optimize-unicorn-workers-in-a-ruby-on-rails-app

we are going to take a look at assembling a multi-layer deployment installation to host Rails based Ruby web applications. For this arrangement, we will use the ever-so-powerful, flexible, and extremely successful Unicorn application server running behind Nginx. Although we will be building this structure on a single server for demonstration purposes, you can easily use multiple droplets to spread things and scale out easily – both horizontally and vertically!


gem install unicorn
# creatig unicron config
vi config/unicorn.rb

# ==================== UNICORN CONFIG
# Set the working application directory
# working_directory "/path/to/your/app"
working_directory "/var/www/my_app"

# Unicorn PID file location
# pid "/path/to/pids/unicorn.pid"
pid "/var/www/my_app/pids/unicorn.pid"

# Path to logs
# stderr_path "/path/to/log/unicorn.log"
# stdout_path "/path/to/log/unicorn.log"
stderr_path "/var/www/my_app/log/unicorn.log"
stdout_path "/var/www/my_app/log/unicorn.log"

# Unicorn socket
listen "/tmp/unicorn.[app name].sock"
listen "/tmp/unicorn.myapp.sock"

# Number of processes
# worker_processes 4
worker_processes 2

# Time-out
timeout 30


# ========================= NGINX CONFIG
# This config files provide the method to reload, restart and perform other actions

# /etc/nginx/conf.d/default.conf
upstream app {
  # Path to Unicorn SOCK file, as defined previously
  server unix:/tmp/unicorn.myapp.sock fail_timeout=0;
}

server {


  listen 80;
  server_name localhost;

  # Application root, as defined previously
  root /root/my_app/public;

  try_files $uri/index.html $uri @app;

  location @app {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_pass http://app;
  }

  error_page 500 502 503 504 /500.html;
  client_max_body_size 4G;
  keepalive_timeout 10;
}

# Nginx managemetn file is in
/etc/nginx/nginx.conf
# this also includes all the files in
/etc/nginx/conf.d/


# this starts an unicorn deamon that runs in the backgrund
# throws an error if already running
unicorn_rails -c config/unicorn.rb -D
service nginx restart


/srv/www



# ====================== COMPRAISON OF APP SERVERS
https://www.digitalocean.com/community/tutorials/a-comparison-of-rack-web-servers-for-ruby-web-applications






#  ============================== CAPISTRANO SCRIPT (config/deploy.rb)
# Define the name of the application
set :application, 'my_app'

# Define where can Capistrano access the source repository
# set :repo_url, 'https://github.com/[user name]/[application name].git'
set :scm, :git
set :repo_url, 'https://github.com/user123/my_app.git'

# Define where to put your application code
set :deploy_to, "/home/deployer/apps/my_app"

set :pty, true

set :format, :pretty

# Set the post-deployment instructions here.
# Once the deployment is complete, Capistrano
# will begin performing them as described.
# To learn more about creating tasks,
# check out:
# http://capistranorb.com/

# namespace: deploy do

#   desc 'Restart application'
#   task :restart do
#     on roles(:app), in: :sequence, wait: 5 do
#       # Your restart mechanism here, for example:
#       execute :touch, release_path.join('tmp/restart.txt')
#     end
#   end

#   after :publishing, :restart

#   after :restart, :clear_cache do
#     on roles(:web), in: :groups, limit: 3, wait: 10 do
#       # Here we can do anything such as:
#       # within release_path do
#       #   execute :rake, 'cache:clear'
#       # end
#     end
#   end

# end



# ========================= config/deploy/production.rb
# Define roles, user and IP address of deployment server
# role :name, %{[user]@[IP adde.]}
role :app, %w{deployer@162.243.74.190}
role :web, %w{deployer@162.243.74.190}
role :db,  %w{deployer@162.243.74.190}

# Define server(s)
server '162.243.74.190', user: 'deployer', roles: %w{web}

# SSH Options
# See the example commented out section in the file
# for more options.
set :ssh_options, {
  forward_agent: false,
  auth_methods: %w(password),
  password: 'user_deployers_password',
  user: 'deployer',
}


# to deploy
cap production deploy
https://www.digitalocean.com/community/tutorials/an-insight-into-the-configuration-of-capistrano-1
https://www.digitalocean.com/community/tutorials/an-insight-into-the-configuration-of-capistrano-2




# ========================== DEPLOYING MULTIPLE RAILS APP
upstream rails1 {
  server 127.0.0.1:8000;
  server 127.0.0.1:8001;
  server 127.0.0.1:8002;
}

upstream rails2 {
  server 127.0.0.1:7000;
  server 127.0.0.1:7001;
  server 127.0.0.1:7002;
}

server {
  location / {
    proxy_pass http://rails1;
  }
  location /app2 {
    proxy_pass http://rails2;
  }
}



# ======================== VPC (virtual private cloud)
individual cluster cloud






# ======================== VPS (virtual private server)
individual server


after installing all the services like postgres, redis and setting up the envionment as root
you should set up a new user other than root
as your base environment is ready you should not allow other programs in the future to change that base
so we set up a new user and give it sudo permission
add user deployer

# =========================== MANAGEMENT SCRIPT
#!/bin/sh
. /etc/rc.d/init.d/functions
. /etc/sysconfig/network
[ "$NETWORKING" = "no" ] && exit 0

nginx="/opt/nginx/sbin/nginx"
prog=$(basename $nginx)

NGINX_CONF_FILE="/opt/nginx/conf/nginx.conf"

lockfile=/var/lock/subsys/nginx

start() {
  [ -x $nginx ] || exit 5
  [ -f $NGINX_CONF_FILE ] || exit 6
  echo -n $"Starting $prog: "
  daemon $nginx -c $NGINX_CONF_FILE
  retval=$?
  echo
  [ $retval -eq 0 ] && touch $lockfile
  return $retval
}

stop() {
  echo -n $"Stopping $prog: "
  killproc $prog -QUIT
  retval=$?
  echo
  [ $retval -eq 0 ] && rm -f $lockfile
  return $retval
}

restart() {
  configtest || return $?
  stop
  start
}

reload() {
  configtest || return $?
  echo -n $”Reloading $prog: ”
  killproc $nginx -HUP
  RETVAL=$?
  echo
}

force_reload() {
  restart
}

configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}

rh_status() {
  status $prog
}

rh_status_q() {
  rh_status >/dev/null 2>&1
}

case "$1" in
  start)
  rh_status_q && exit 0
  $1
  ;;
  stop)
  rh_status_q || exit 0
  $1
  ;;
  restart|configtest)
  $1
  ;;
  reload)
  rh_status_q || exit 7
  $1
  ;;
  force-reload)
  force_reload
  ;;
  status)
  rh_status
  ;;
  condrestart|try-restart)
  rh_status_q || exit 0
  ;;
  *)
  echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
  exit 2
  esac
