# unicorn vs unicorn_rails
unicorn_rails has been modeled for ruby applications
But it is not required
It is designed to help Rails 1.x and 2.y users transition to Rack, but it is NOT needed for Rails 3 applications. Rails 3 users are encouraged to use unicorn(1) instead of unicorn_rails(1). Users of Rails 1.x/2.y may also use unicorn(1) instead of unicorn_rails(1).
unicorn_rails is not a gem but just an additional bin command provided by the unicorn gem



http://nicolai86.eu/blog/posts/2012-11-28/zero-downtime-deployments-with-unicorn-and-supervisord/

http://2ndscale.com/rtomayko/2009/unicorn-is-unix
https://blog.engineyard.com/2010/everything-you-need-to-know-about-unicorn
https://www.digitalocean.com/community/tutorials/how-to-optimize-unicorn-workers-in-a-ruby-on-rails-app
http://sirupsen.com/setting-up-unicorn-with-nginx/
http://stackoverflow.com/questions/18575235/what-do-multi-processes-vs-multi-threaded-servers-most-benefit-from
http://unicorn.bogomips.org/PHILOSOPHY.html

Unicorn
http://underthehood.meltwater.com/blog/2014/03/21/debugging-unicorn-rails-timeouts/
https://www.digitalocean.com/community/tutorials/how-to-optimize-unicorn-workers-in-a-ruby-on-rails-app
http://vstark.net/2012/10/21/nginx-unicorn-performance-tweaks/


get status of unicorn process
It may be that a process is going in infinite loop and thats why there is an issue

https://www.nateberkopec.com/2015/07/29/scaling-ruby-apps-to-1000-rpm.html
https://blog.newrelic.com/2013/04/29/debugging-stuck-ruby-processes-what-to-do-before-you-kill-9/

# ======================== UNICORN HERDER
"Unicorn Herder is a utility designed to assist in the use of Upstart and similar supervisors with Unicorn. It does this by polling the pidfile written by the Unicorn master process, and automating the sequence of signals that must be sent to the master to do a "hot-reload". If Unicorn quits, so will the Unicorn Herder, meaning that if you supervise the herder (which does not daemonize), you are effectively supervising the Unicorn process."
http://www.dangtrinh.com/2014/12/deploy-redmine-26-with-unicorn.html

# ==================== Running uicorn as root
As Thomas said in sec.se chat, running anything as root carries an implicit security risk. The thing to understand about root is that the kernel essentially trusts all its actions without complaint.

The issue occurs if there are any vulnerabilities in nginx, or unicorn. If this happens, it may be possible to execute an exploit, misusing the process.
http://unix.stackexchange.com/questions/25822/debian-nginx-unicorn-permissions



###################################################################################################################################
# ############################################### BASICS
#######################################################################################################################################

unciron is process based whereas puma is thread based
Each forking of worker in unicorn is a seperate process as you can see in the ps aux they have seperate pid
Also master has control over these worker processes. When you kill the master they will too die (they follow master)
If you kill one worker process master will respawn it.
after_fork and before_fork hooks are meant to execute custom code before and after forking
For a forking server such as Unicorn, the master process will boot your rails applications (and execute any initializers) and then fork workers.
For this reason it’s necessary to disconnect in your master process in the before_fork and then re-establish the connection in an after_fork block.
Unicorn is single threaded multi process (fork)
Puma is single process multi threaded

before_fork do |server, worker|
# other settings
  if defined?(ActiveRecord::Base)
    ActiveRecord::Base.connection.disconnect!
  end
end

after_fork do |server, worker|
# other settings
  if defined?(ActiveRecord::Base)
    ActiveRecord::Base.establish_connection
  end
end


# preload_app directive
The preload_app directive loads the application before forking workers, so it can share any loaded data structures. By default, workers each load a private copy of their app for out-of-the-box compatibility with existing servers.

# init script syntax
/etc/init.d/skeleton as a template

### BEGIN INIT INFO
# Provides:          FOO
# Required-Start:    $syslog $remote_fs
# Required-Stop:     $syslog $remote_fs
# Default-Start:     3 5
# Default-Stop:      0 1 2 6
# Description:       Start FOO to allow XY and provide YZ
### END INIT INFO


If you are using Rails 4.1+ then ActiveRecord::Base.establish_connection will use the connection information stored in config/database.yml. Otherwise you will need to duplicate the behavior in your initializer to ensure consistent connection information:
Note we setting the pool to 5 connections or the value specified in the DB_POOL env var. Now you can set the connection pool size by setting a config var on Heroku.
This doesn’t mean that each dyno will now have 10 open connections, but only that if a new connection is needed it will be created until a maximum of 10 have been used per Rails process.
In heroku number of dynos are the number of unicorn processes
6 dynos = 6 unicorn master workers having their own slave workers

# Maximum database connections in postgres
Heroku provides managed Postgres databases. Different tiered databases have different connection limits. The Starter Tier “Dev” and “Basic” databases are limited to 20 connections. Production Tier databases (plans Crane and up) have higher limits. Once your database has the maximum number of active connections, it will no longer accept new connections. This will result in connection timeouts from your application and will likely cause exceptions.
When scaling out, it is important to keep in mind how many active connections your application needs. If each dyno allows 5 database connections, you can only scale out to four dynos before you need to provision a more robust database.

# Limit connections with PgBouncer
You can continue to scale out your applications with additional dynos until you have reached your database connection limits. Before you reach this point it is recommended to limit the number of connections required by each dyno by using the PgBouncer buildpack.

# Bad connections
It is possible for connections to hang, or be placed in a “bad” state. This means that the connection will be unusable, but remain open. If you are running a multi-process web server such as Unicorn this could mean that over time a 3 worker dyno which normally consumes 3 database connections could be holding as many as 15 connections (5 default connections per pool times 3 workers). To limit this threat lower the connection pool to 1 or 2 and enable connection reaping which is available in Rails 4, though it was turned off by default after this bug report


# Number of active connections
# In development you can see the number of connections taken up by your application by checking the database.
$ bundle exec rails dbconsole
# This will open a connection to your development database. You can then see the number of connections to your postgres 9.1 or previous database by running:
select count(*) from pg_stat_activity where procpid <> pg_backend_pid() and usename = current_user;
# On Postgres 9.2 and later the command is:
select count(*) from pg_stat_activity where pid <> pg_backend_pid()  and usename = current_user;
# Which will return with the number of connections on that database:
Since connections are opened lazily, you’ll need to hit your running application at localhost several times until the count quits going up. To get an accurate count you should run that database query inside of a production database since your development setup may not allow you to generate load required for your app to create new connections.


###################################################################################################################################
# ############################################### CONTROLLING
#######################################################################################################################################


# without using init.d scripts
Alternatively, instead of relying on /etc/init.d... scripts which are OS dependent, a simple way to restart unicorn is to send HUP (1) signal to its master process.

unicorn_pid=`cat /tmp/pids/unicorn.pid`
echo "Restarting Unicorn ($unicorn_pid)"
kill -HUP $unicorn_pid

# signals can be sent to the master process to control
INT/TERM - quick shutdown, kills all workers immediately

QUIT - graceful shutdown, waits for workers to finish their current request before finishing.

USR1 - reopen all logs owned by the master and all workers See Unicorn::Util.reopen_logs for what is considered a log.

USR2 - reexecute the running binary. A separate QUIT should be sent to the original process once the child is verified to be up and running.

WINCH - gracefully stops workers but keep the master running. This will only work for daemonized processes.

TTIN - increment the number of worker processes by one

TTOU - decrement the number of worker processes by one

HUP - reloads config file and gracefully restart all workers.

If the "preload_app" directive is false (the default), then workers will also pick up any application code changes when restarted.

# Send singlars to the workers
Sending signals directly to the worker processes should not normally be needed.


# Replacing a running unicorn executable
You may replace a running instance of unicorn with a new one without losing any incoming connections. Doing so will reload all of your application code, Unicorn config, Ruby executable, and all libraries. The only things that will not change (due to OS limitations) are:

The path to the unicorn executable script. If you want to change to a different installation of Ruby, you can modify the shebang line to point to your alternative interpreter.

The procedure is exactly like that of nginx:

Send USR2 to the master process

Check your process manager or pid files to see if a new master spawned successfully. If you're using a pid file, the old process will have ".oldbin" appended to its path. You should have two master instances of unicorn running now, both of which will have workers servicing requests. Your process tree should look something like this:

unicorn master (old)
\_ unicorn worker[0]
\_ unicorn worker[1]
\_ unicorn worker[2]
\_ unicorn worker[3]
\_ unicorn master
\_ unicorn worker[0]
\_ unicorn worker[1]
\_ unicorn worker[2]
\_ unicorn worker[3]
You can now send WINCH to the old master process so only the new workers serve requests. If your unicorn process is bound to an interactive terminal (not daemonized), you can skip this step. Step 5 will be more difficult but you can also skip it if your process is not daemonized.

You should now ensure that everything is running correctly with the new workers as the old workers die off.

If everything seems ok, then send QUIT to the old master. You're done!

If something is broken, then send HUP to the old master to reload the config and restart its workers. Then send QUIT to the new master process.



###################################################################################################################################
# ############################################### UNCIRON WORKING
#######################################################################################################################################

# ================================== DESIGN
Simplicity: Unicorn is a traditional UNIX prefork web server. No threads are used at all, this makes applications easier to debug and fix. When your application goes awry, a BOFH can just "kill -9" the runaway worker process without worrying about tearing all clients down, just one. Only UNIX-like systems supporting fork() and file descriptor inheritance are supported.
The Ragel+C HTTP parser is taken from Mongrel. This is the only non-Ruby part and there are no plans to add any more non-Ruby components.
All HTTP parsing and I/O is done much like Mongrel:
1. read/parse HTTP request headers in full
2. call Rack application
3. write HTTP response back to the client
   If the master process dies unexpectedly for any reason, workers will notice within :timeout/2 seconds and follow the master to its death.

Unicorn makes great use of unix
# ============================== CONNECTION BETWEEN NGINX AND UNICORN
nginx is http server whereas unicorn is the app server
socket vs listening to 9000 port
unicorn can create sockets or run on a http port
the best way is using socket
once unicorn creates a socket file the nginx writes to the socket file
Unicorn READS the socket file whereas NGINX writes to the file

NGINX creates a user of its own
nginx user must be able to write to the socket file
we have to change the ownership of this socket file so that nginx can write


# another method
Client --------- Nginx (port 80) ----------- Unicorn (port 8080 reverse proxy) ----------------- supervisordd ----------- redis
                                              /              |              \
                                        web worker      web worker         web worker


unciorn accepts connections in queue
While processing each element in queue it checks wehter client is still connected
if connected
  rails application processes request and renders result
  result written to socket and connection closed

Unicorn can bind to ports or sockets whichever method preferend

# binding to a port
unicorn can bind to a port
here the nginx (at port 80) will act as a reverse proxy to allow the outside to access the inside ports at port 80
keep the port of unicorn > 1024 as ports < 1024 are used by system (preference is given to them)
To be safe use > 3000
However it is good to do this using socket

# Ports vs Sockets
Unix sockets are a little bit faster as you dont have the tcp-overhead.
If you realize this performance loss is a question of server load. If you dont have very high server load you wont recognize it.
You need to find out if your system is under heavy load so that a socket is a must or you can focus on a nice system design (separating services), then a tcp/ip solution would be better.
Beware though that sockets are only reachable from programs that are running on the same server (there no network support, obviously) and that the programs need to have the necessary permissions to access the socket file.
Genrally in case of single app single machine use socks. In case of multiple apps ports can be used as they are easier to maintain.


# Sockets
Unicorn binds to unix socket where nginx dumps requests
time out tells how long should unicorn wait for the worker to work till timeout
Recommended worker count = (no. of cores * 1.5).round
Pid file is just used to tell wehter unicorn is running and give its process id. Nothing else.

# Shared memory IPC
A Unix domain socket uses the local file system to create an IPC mechanism between the server and client processes.
You will see a file in /var somewhere when the Unix domain socket is connected.
If you are looking for purely the ultimate performance solution you may want to explore a shared memory IPC.

# forking
runs a master process that forks into N workers that are specified
- Master process runs the before fork
- Each workers run the after fork block
- Master monitors all the workers for stability
- Kills anything that doesnt respon in 30secs and then forks another to replace that.
- All workers use the socket file to pull. Nginx pushes to the socket.
- Best for one app on hardware usecase
- in memory forking and fast client request execution
- Zero downtime deployments using QUIT+SIGURS2 signal. Reload master which then kills old workers and forks itself again.
- Kills workers running long requests bad choice for web sockets/long polling
- It has been engineered to tie with the OS closely and boost performance









############################################################################################################################################################
# ################################# UNCIRON DEPLOYMENTS AND MONITORING####################################################
############################################################################################################################################################
# =============================== DEPLOYMENT
Today we have to clone git repositories, restart servers, set permissions, create symlinks to our configuration files, clean out caches and what not.
Deployments are slow and cause downtime
Fast and continuos deplyments



# ================================= Zero Downtime deployments running unircorn as a service
Unicron can be run as a service by speciying this in an init script
there are many t

runit - a UNIX init scheme with service supervision

we need to manage how to stop, restart etc. the unicorn service correctly



# =============== Using runit
One way of turning unix into a service is using runit
the git for example uses runit
runit - a UNIX init scheme with service supervision





# ================================== Basics and comparison
Thin and webrick acts as both app and HTTP server. Both lightewight


On changes in the application unicorn restart is needed not nginx restart
nginx not dependent on application
nginx restart needed when port changed etc.







##################################################################################################################################################
# #################################################### DIFFERENCE
###############################################################################################################################################
Passenger
- super easy configuration. Very Easy
- embeds itself into nginx/apache.
- Excels in running multiple applicatinos at same time. Best at multiple apps on same hardware
- Rolling restarts supported (enterprise version). Capable of resetting to older version after buggy deploy.
- Can route requests to specific workers based on their load
- can prestart app workers by spoofing a request to app domain.
- all config in nginx config file
- That is why it is good for staging type environment.
  Thin
- Even Machine based application server
- one socket per worker. Configure nginx to round robing amongst the workers.
- Can work well with long runing request as is event based. Eventmachine based. Web sockets, live streaming etc.
- Can perform rolling restarts
- single application on hardware
- Nginx load balances with defintion in upstream config.
  Puma
- Concurrent request processing

High CPU Medium Amazon EC2 stack - 2 virtual CPU and 1.7GB memory






##################################################################################################################################################
# #################################################### CONFIGURATION####################################################
###############################################################################################################################################
# Basic configuratoin
listen 2007 # by default Unicorn listens on port 8080
worker_processes 2 # this should be >= nr_cpus
pid "/path/to/app/shared/pids/unicorn.pid"
stderr_path "/path/to/app/shared/log/unicorn.log"
stdout_path "/path/to/app/shared/log/unicorn.log"


# global configuration file
make a file in /etc/unicorn

default.rb

# app specific configuration
config/unicorn.rb
https://github.com/gitlabhq/gitlabhq/blob/master/config/unicorn.rb.example

worker_processes 3
working_directory "/home/git/gitlab"
listen "/home/git/gitlab/tmp/sockets/gitlab.socket", :backlog => 1024
timeout 60
pid "/home/git/gitlab/tmp/pids/unicorn.pid"

# ===================== STARTING UNICORN PROCESS as a DAEMON (runs in the bacground always)

# Start Rails app with Unicorn using
unicorn_rails -c /path/to/app/config/unicorn.conf.rb

# To start it as a daemon
unicorn_rails -c /path/to/app/config/unicorn.conf.rb -D

# To start it as a daemon in production environment
unicorn_rails -c /path/to/app/config/unicorn.conf.rb -E production -D



# ====================== CONFIURING UNICORN DAEMON AS SERVICE
we can also configure it as a service so that we can easily start/stop it


# Once started as a daemon it runs as a background task. To stop unicorn server we must manually kill the process.
# Lists all processes
ps -ef

# Find the pid of unicorn
kill <process_master>

# ========================== LOGS
nginx.access.log
nginx.error.log
unicorn.stdout.log
unicorn.stderr.log




# ======================== CONFIGRUATIONS

after_fork (*args, &block)

tmeout
sterr_path
stdout_path



user (user, group = nil) source
# Runs worker processes as the specified user and group. The master process always stays running as the user who started it.
# This switch will occur after calling the #after_fork hook, and only if the Worker#user method is not called in the #after_fork hook group is optional and will not change if unspecified.

worker_processes

working_directory

timeout
# For running Unicorn behind nginx, it is recommended to set "fail_timeout=0" for in your nginx configuration like this to have nginx always retry backends that may have had workers SIGKILL-ed due to timeouts.

pid (path) source
# sets the path for the PID file of the unicorn master process

preload_app (bool) source
# Enabling this preloads an application before forking worker processes. This allows memory savings when using a copy-on-write-friendly GC but can cause bad things to happen when resources like sockets are opened at load time by the master process and shared by multiple children. People enabling this are highly encouraged to look at the before_fork/after_fork hooks to properly close/reopen sockets. Files opened for logging do not have to be reopened as (unbuffered-in-userspace) files opened with the File::APPEND flag are written to atomically on UNIX.
# In addition to reloading the unicorn-specific config settings, SIGHUP will reload application code in the working directory/symlink when workers are gracefully restarted when #preload_app=false (the default). As reloading the application sometimes requires RubyGems updates, Gem.refresh is always called before the application is loaded (for RubyGems users).
# During deployments, care should always be taken to ensure your applications are properly deployed and running. Using #preload_app=false (the default) means you must check if your application is responding properly after a deployment. Improperly deployed applications can go into a spawn loop if the application fails to load. While your children are in a spawn loop, it is is possible to fix an application by properly deploying all required code and dependencies. Using #preload_app=true means any application load error will cause the master process to exit with an error.










# NGINX is the single point of contact for the client that lands.
# NGINx


# Running multple NGINx use cases. A single NGINx can hanle around 5k req/secs. If there are more than this as in case of wordrpess then we need to scale out.



web server communicated with app server using CGI (common gateway interface)

# ==================== UNCICORN vs THIN (both are web servers)
# Unicorn is an HTTP server for Ruby, similar to Mongrel or Thin. It uses Mongrel’s Ragel HTTP parser but has a dramatically different architecture and philosophy.


# All the server part is handled by the rack middleware
# rack is a middleware written in ruby
# app frameworks that can adopt rack middleware are called rack based apps.

# In the classic setup you have nginx sending requests to a pool of mongrels using a smart balancer or a simple round robin.

# Eventually you want better visibility and reliability out of your load balancing situation, so you throw haproxy into the mix:

As the guys from Github point out, Nginx -> Unicorn is a good alternative to Nginx -> HA Proxy -> Other backends (Mongrel, Thin, Passenger). The role of HA Proxy behind Nginx was to health check the backend cluster and make sure it is ready to connections. What’s nice about Unicorn is that the backend cluster (worker processes) are pulling requests from a shared socket as opposed to being pushed requests from a load balancer.
https://github.com/blog/517-unicorn


WordPress.com customer data is instantly replicated between different locations to provide an extremely reliable and fast web experience for hundreds of millions of visitors.

It was soon moved to a single dedicated server and then to two servers. In late 2005, WordPress.com opened to the public and by early 2006 had expanded to four web servers, with traffic being distributed using round robin DNS. Soon thereafter WordPress.com expanded to a second data center and then to a third. It quickly became apparent that round robin DNS wasn’t a viable long-term solution.

# =========================== managing uniocorn processes
Simple Things There - In Terminal type "ps" and have a look for the Master Unicorn Process. Copy the PID of it and then type "kill −9 90234" (where 90234 is PID of master unicorn process). After that worker process should disappear itself.

/etc/init.d/unicorn_myapp restart
/etc/init.d/unicorn_myapp stop
/etc/init.d/unicorn_myapp start


service unicorn_myapp restart

service unicorn_myapp stop
service unicorn_myapp start

# ============================ OPTIONS
Ruby options:
-e, --eval LINE          evaluate a LINE of code
-d, --debug              set debugging flags (set $DEBUG to true)
-w, --warn               turn warnings on for your script
-I, --include PATH       specify $LOAD_PATH (may be used more than once)
-r, --require LIBRARY    require the library, before executing your script

unicorn options:
-o, --host HOST          listen on HOST (default: 0.0.0.0)
-p, --port PORT          use PORT (default: 8080)
-E, --env RACK_ENV       use RACK_ENV for defaults (default: development)
-N                       do not load middleware implied by RACK_ENV
--no-default-middleware
-D, --daemonize          run daemonized in the background

-s, --server SERVER      this flag only exists for compatibility
-l {HOST:PORT|PATH},     listen on HOST:PORT or PATH
--listen             this may be specified multiple times
(default: 0.0.0.0:8080)
-c, --config-file FILE   Unicorn-specific config file



# =================== POUND LOAD BALANCING
At first, the WordPress.com team chose Pound as a software load balancer because of its ease of use and built-in SSL support. After using Pound for about two years, WordPress.com required additional functionality and scalability, namely:

On-the-fly reconfiguration capabilities, without interrupting live traffic.
Better health check mechanisms, allowing to smoothly and gradually recover from a backend failure, without overloading application infrastructure with unexpected load of requests.
Better scalability both requests per second, and the number of concurrent connections. Pound’s thread-based model wasn’t able to reliably handle over 1,000 requests per second per load balancing instance.


# ================= Features of NGINX
Easy, flexible and logical configuration.
Ability to reconfigure and upgrade NGINX instances on-the-fly, without dropping user requests.
Application request routing via FastCGI, uwsgi or SCGI protocols; NGINX can also serve static content directly from storage for additional performance optimization.
The only software tested that was capable of reliably handling over 10,000 request per second of live traffic to WordPress applications from a single server.
NGINX’s memory and CPU footprints are minimal, and predictable. After switching to NGINX the CPU usage on the load balancing servers dropped three times.

# ============== MEMORY PERFORMANCE









# ======================= REAL WORLD TESTING
Thin, one process: 3638 requests / second
Thin, three processes: 4347 requests / second
Unicorn, one worker: 6774 requests / second
Unicorn, three workers: 8,012 requests / second
Conclusion
Clearly, Unicorn gives some nice performance benefits. It also seems a lot more flexible, as you don’t even need to use something like nginx to load balance between worker processes — it ties in directly with the operating system and load balances itself, which obviously lent to the performance of it.





###################################################################################################################################
# ############################################### MONITORING
#######################################################################################################################################
One of the most amazing things you can do with Unicorn is monitor its behavior in depth with ease and peace of mind (read UNIX Signals).
To monitor a Unicorn process you can use a multitude of tools: bluepill, god and monit. The first two being Ruby applications while monit being a C application.

Since we read so many horror stories about Ruby memory leaking all over that great unicorn fur, we chose monit as our main monitoring solution (not that its syntax is so much more complex than Ruby’s).













worker_processes ((ENV['RAILS_ENV'] == 'development') ? 2 : 8)

working_directory ENV["APP_PATH"]

listen ENV["PORT"].to_i, :tcp_nopush => true


# ==================================== SAMPLE CONFIG WITH PRELOAD APP TRUE
worker_processes 2
working_directory "/var/rails/testapp/"

# This loads the application in the master process before forking
# worker processes
# Read more about it here:
# http://unicorn.bogomips.org/Unicorn/Configurator.html
preload_app true

timeout 30

# This is where we specify the socket.
# We will point the upstream Nginx module to this socket later on
listen "/var/rails/testapp/tmp/sockets/unicorn.sock", :backlog => 64

pid "/var/rails/testapp/tmp/pids/unicorn.pid"

# Set the path of the log files inside the log folder of the testapp
stderr_path "/var/rails/testapp/log/unicorn.stderr.log"
stdout_path "/var/rails/testapp/log/unicorn.stdout.log"

before_fork do |server, worker|
# This option works in together with preload_app true setting
# What is does is prevent the master process from holding
# the database connection
  defined?(ActiveRecord::Base) and
  ActiveRecord::Base.connection.disconnect!
end

after_fork do |server, worker|
# Here we are establishing the connection after forking worker
# processes
  defined?(ActiveRecord::Base) and
  ActiveRecord::Base.establish_connection
end

# ================================= CONFIG WITH INSTRUCTIONS
worker_processes 3

# Since Unicorn is never exposed to outside clients, it does not need to
# run on the standard HTTP port (80), there is no reason to start Unicorn
# as root unless it's from system init scripts.
# If running the master process as root and the workers as an unprivileged
# user, do this to switch euid/egid in the workers (also chowns logs):
# user "unprivileged_user", "unprivileged_group"

# Help ensure your application will always spawn in the symlinked
# mina sets it up in the current directory. Using app specific working directory as there may be multiple apps on the systems
working_directory "/srv/www/amber.frontend/current" # available in 0.94.0+

# Listen on both a Unix domain socket and a TCP port.
# If you are load-balancing multiple Unicorn masters, lower the backlog
# setting to e.g. 64 for faster failover.
listen "/tmp/unicorn.amber.frontend.sock", :backlog => 1024
# listen "127.0.0.1:8080", :tcp_nopush => true

# Nuke workers after 30 secs
timeout 30

# feel free to point this anywhere accessible on the filesystem
pid "/srv/www/amber.frontend/shared/tmp/unicorn.pid"

# directories for logging
stderr_path "/srv/www/amber.frontend/shared/log/unicorn.stderr.log"
stdout_path "/srv/www/amber.frontend/shared/log/unicorn.stdout.log"

# combine Ruby 2.0.0dev or REE with "preload_app true" for memory savings
# http://rubyenterpriseedition.com/faq.html#adapt_apps_for_cow
# preload_app true
# GC.respond_to?(:copy_on_write_friendly=) and
# GC.copy_on_write_friendly = true

# Enable this flag to have unicorn test client connections by writing the
# beginning of the HTTP headers before calling the application.  This
# prevents calling the application for connections that have disconnected
# while queued.  This is only guaranteed to detect clients on the same
# host unicorn runs on, and unlikely to detect disconnects even on a
# fast LAN.
# check_client_connection false

# before_fork do |server, worker|
# the following is highly recomended for Rails + "preload_app true"
# as there's no need for the master process to hold a connection
# defined?(ActiveRecord::Base) and
# ActiveRecord::Base.connection.disconnect!

# The following is only recommended for memory/DB-constrained
# installations.  It is not needed if your system can house
# twice as many worker_processes as you have configured.

# This allows a new master process to incrementally
# phase out the old master process with SIGTTOU to avoid a
# thundering herd (especially in the "preload_app false" case)
# when doing a transparent upgrade.  The last worker spawned
# will then kill off the old master process with a SIGQUIT.
# old_pid = "#{server.config[:pid]}.oldbin"
# if old_pid != server.pid
# begin
# sig = (worker.nr + 1) >= server.worker_processes ? :QUIT : :TTOU
# Process.kill(sig, File.read(old_pid).to_i)
# rescue Errno::ENOENT, Errno::ESRCH
# end
# end

# Throttle the master from forking too quickly by sleeping.  Due
# to the implementation of standard Unix signal handlers, this
# helps (but does not completely) prevent identical, repeated signals
# from being lost when the receiving process is busy.
# sleep 1
# end

# after_fork do |server, worker|
# per-process listener ports for debugging/admin/migrations
# addr = "127.0.0.1:#{9293 + worker.nr}"
# server.listen(addr, :tries => -1, :delay => 5, :tcp_nopush => true)

# the following is *required* for Rails + "preload_app true",
# defined?(ActiveRecord::Base) and
# ActiveRecord::Base.establish_connection

# if preload_app is true, then you may also want to check and
# restart any other shared sockets/descriptors such as Memcached,
# and Redis.  TokyoCabinet file handles are safe to reuse
# between any number of forked children (assuming your kernel
# correctly implements pread()/pwrite() system calls)
# end








Skip to content
This repository
Explore
Gist
Blog
Help
@saurgoel saurgoel

Watch 7
Star 20
Fork 7scarfacedeb/mina-unicorn
branch: master  mina-unicorn/lib/mina/unicorn/utility.rb
@ridiculousridiculous on Jan 8 echo more like mina
2 contributors @scarfacedeb @ridiculous
RawBlameHistory     110 lines (91 sloc)  2.738 kb
# Ported from: https://github.com/sosedoff/capistrano-unicorn/blob/master/lib/capistrano-unicorn/utility.rb

module Mina
  module Unicorn
    module Utility

      # Check if a remote process exists using its pid file
      #
      def remote_process_exists?(pid_file)
        "[ -e #{pid_file} ] && kill -0 `cat #{pid_file}` > /dev/null 2>&1"
      end

      # Stale Unicorn process pid file
      #
      def old_unicorn_pid
        "#{unicorn_pid}.oldbin"
      end

      # Command to check if Unicorn is running
      #
      def unicorn_is_running?
        remote_process_exists?(unicorn_pid)
      end

      # Command to check if stale Unicorn is running
      #
      def old_unicorn_is_running?
        remote_process_exists?(old_unicorn_pid)
      end

      # Get unicorn master process PID (using the shell)
      #
      def get_unicorn_pid(pid_file=unicorn_pid)
        "`cat #{pid_file}`"
      end

      # Get unicorn master (old) process PID
      #
      def get_old_unicorn_pid
        get_unicorn_pid(old_unicorn_pid)
      end

      # Send a signal to a unicorn master processes
      #
      def unicorn_send_signal(signal, pid=get_unicorn_pid)
        "kill -s #{signal} #{pid}"
      end

      # Kill Unicorns in multiple ways O_O
      #
      def kill_unicorn(signal)
        script = <<-END
        if #{unicorn_is_running?}; then
          echo "-----> Stopping Unicorn...";
          #{unicorn_send_signal(signal)};
        else
          echo "-----> Unicorn is not running.";
          fi;
          END

          script
        end

        # Start the Unicorn server
        #
        def start_unicorn
          %Q%
          if [ -e "#{unicorn_pid}" ]; then
            if kill -0 `cat #{unicorn_pid}` > /dev/null 2>&1; then
              echo "-----> Unicorn is already running!";
              exit 0;
              fi;
              rm #{unicorn_pid};
              fi;
              echo "-----> Starting Unicorn...";
              cd #{deploy_to}/#{current_path} && BUNDLE_GEMFILE=#{bundle_gemfile} #{unicorn_cmd} -c #{unicorn_config} -E #{unicorn_env} -D;
              %
            end

            # Restart the Unicorn server
            #
            def restart_unicorn
              %Q%
              #{duplicate_unicorn}
              sleep #{unicorn_restart_sleep_time}; # in order to wait for the (old) pidfile to show up
              if #{old_unicorn_is_running?}; then
                #{unicorn_send_signal('QUIT', get_old_unicorn_pid)};
                fi;
                %
              end

              def duplicate_unicorn
                %Q%
                if #{unicorn_is_running?}; then
                  echo "-----> Duplicating Unicorn...";
                  #{unicorn_send_signal('USR2')};
                else
                  #{start_unicorn}
                  fi;
                  %
                end

              end
            end
          end
          Status API Training Shop Blog About
          © 2015 GitHub, Inc. Terms Privacy Security Contact
          