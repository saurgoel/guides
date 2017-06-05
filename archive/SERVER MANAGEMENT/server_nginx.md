# The server nginx can use the sock for communications
# it is a faster way to communicate
# nginx writes to a sock file and unicorn reads from that sock file

# Regoarding the directory of socket file
# You can't place sockets intended for interprocess communication in /tmp.

# ==================================== Biggest Problem with starting nginx
http://stackoverflow.com/questions/27760400/sudo-nginx-vs-sudo-service-nginx-start


# ======================================== server_name directive
# nginx first decides which server should process the request. Let’s start with a simple configuration where all three virtual servers listen on port *:80:
# In this configuration nginx tests only the request’s header field “Host” to determine which server the request should be routed to.
# Here each server block is a virtual server
# Host is added for each of the request that is being made
# For examlple if you are making a request to http://getamber.com/some_api then getamber.com is the host

server {
    listen      80;
    server_name example.org www.example.org;
    ...
}

server {
    listen      80;
    server_name example.net www.example.net;
    ...
}

server {
    listen      80;
    server_name example.com www.example.com;
    ...
}



The server name directive can be used to filter out resources that are coming from a specific source
For example we can add server_name to be .getamber.com to filter results that are coming from that host

DNS SERVER -> 

Server names are defined using the server_name directive and determine which server block is used for a given request. See also “How nginx processes a request”. They may be defined using exact names, wildcard names, or regular expressions:

# 1. When searching for a virtual server by name, if name matches more than one of the specified variants, e.g. both wildcard name and regular expression match, the first matching variant will be chosen, in the following order of precedence: exact name
# 2. longest wildcard name starting with an asterisk, e.g. “*.example.org”
# 3. longest wildcard name ending with an asterisk, e.g. “mail.*”
# 4. first matching regular expression (in order of appearance in a configuration file)




# ======================================== Use of @
# @ defines a resource that we can use to redirect
# For instance in the following example we can use it to route multiple apps
# It can perform number of operations
# @app gets referenced in location @app

try_files $uri/index.html $uri.html $uri @app;

location @app {
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header Host $host;
  proxy_set_header  Client-IP "";
  proxy_pass http://login_app_server;
}

error_page 500 502 503 504 /500.html;

error_page 503 @maintainence;

location @maintainence {
  rewrite ^(.*)$ /maintainence.html break;
}


# ======================================== redirects
Redirect non-www to WWW
Single domain

server {
        server_name example.com;
        return 301 $scheme://www.example.com$request_uri;
}
All domains

server {
        server_name "~^(?!www\.).*" ;
        return 301 $scheme://www.$host$request_uri;
}
From WWW to non-WWW
Single domain

server {
        server_name www.example.com;
        return 301 $scheme://example.com$request_uri;
}
All domains

server {
         server_name "~^www\.(.*)$" ;
         return 301 $scheme://$1$request_uri ;
}





Advanced Nginx
https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching
Pound and varnish
http://edoceo.com/howto/nginx-varnish-ssl
https://rtcamp.com/blog/why-we-never-use-varnish-with-nginx/
http://www.narga.net/varnish-nginx-comparison-nginx-alone-better/
http://wptavern.com/speed-up-your-wordpress-site-with-pound-varnish-nginx-and-mod_pagespeed

# ==================== 403 errors (even after fixing permissios)
SE linux problems

use sudo nginx to start nginx. Everythng works fine
sudo nginx
http://stackoverflow.com/questions/22586166/why-does-nginx-return-a-403-even-though-all-permissions-are-set-properly#answer-26228135

# ===================== Ip Tables

# 8. Open iptables using the following command
# vi   /etc/sysconfig/iptables
# 9. Add the following line to the file
# -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
# 10. save the file and restart the iptables
# service iptables restart

# ======================== USING ENVIRONMENT VARIABLES
you have to enable the perl or lua modules
https://docs.apitools.com/blog/2014/07/02/using-environment-variables-in-nginx-conf.html

# ====================== Modifying the source and compiling
https://www.digitalocean.com/community/tutorials/how-to-customize-your-nginx-server-name-after-compiling-from-source-in-centos
You can stop it outputting the version of Nginx by adding

server_tokens off;
Or if you want to remove the Server header completely, you need to compile Nginx with the Headers More module in, as the header is hard coded in the Nginx source, and this module allows changing any http headers.

more_clear_headers Server;


# =================== limiting access
Setup host access
We need to setup host access (TCP_WRAPPERS).  There are two host access files (/etc/hosts.allow and /etc/hosts.deny) that are part of the TCP_WRAPPER package.  This makes it possible to allow or deny access to certain services based on the IP.  We need to edit the hosts.allow and hosts.deny files:

Run the following command to edit the file in vi text editor:
# vi /etc/hosts.allow

Paste the following into the file and then save the file:
sshd:ALL
vsftpd:ALL
sendmail:ALL


Tip: I recommend limiting access to your production servers based on specific IP addresses.  This helps to prevent intruders from getting access to your server.
For example, lets say you have an office IP address of 55.55.55.55, you would make the sshd line look like this:
sshd:55.55.55.55

###################################################################################################################################
# ############################################### BASICS ####################################################
#######################################################################################################################################
Nginx is very very lightweight that makes it very very fast
it can route requests like nothing else
it can run in nested mode also
Nginx is a webserver like apache. Performance wise, nginx is considered to be better than apache. Processing request in Nginx is even based as opposed to the spawning new thread model in apache.

nginx has built in load balancing
there is also amazon elb - elastic load balancer that uses Hap proxy (hyper proxy) that is also a load balancer

in housing a single nginx serves to 6 nginx (with load balancing) where each nginx serves to 4 unicorn workers

https://www.digitalocean.com/community/tutorials/how-to-configure-logging-and-log-rotation-in-nginx-on-an-ubuntu-vps

# nginx is written in C

###################################################################################################################################
# ############################################### EXTENSIONS ####################################################
#######################################################################################################################################
Nginx has built in load balancing. But there are some other programs that are specialised for this available.
Why muck around with HAProxy and Varnish when you can have nginx do it all for you.
http://www.nathanvangheem.com/news/nginx-with-built-in-load-balancing-and-caching

# Nginx internal load balancing vs haproxy for load balancing
HAProxy is a superior load balancer to nginx. HAProxy can do out-of-band health checks, whereas nginx only knows a backend to be "down" when it serves a 500. Also, HAProxy is a general TCP load balancer, whereas nginx will work only on HTTP traffic.
Yes, HAProxy would be better one if you needs intligenence proxy layer and security for your backend stack.
HAproxy is simpler (being just a tcp proxy without an http implementation), which makes it faster and less bug prone.

if you need to scale your backend server, you can easily use something like pound/haproxy between the frontend (static-serving) HTTP and your backends (Zope is often deployed like that);
it can be a nice sidekick if you are also using some kind of outward-facing, caching, reverse proxy (like Varnish or Squid) since it allows to bypass it very easily.

# Nginx internal caching vs Varnish

Varnish is not a tool for connection managment, its a tool to cache web-pages and make them faster.


# Caching
When request comes from the front end, several parts of the web pages can be cached
First we hit the cache servers (varnish squid). If they have the cache present they serve it otherwise we hit the backend servers and get a cache ready.


###################################################################################################################################
# ############################################### CONTROLLING ####################################################
#######################################################################################################################################
NGINX has one master process and one or more worker processes. If caching is enabled, the cache loader and cache manager processes (seperate) will also run.
The main purpose of the master process is to read and evaluate configuration files, as well as, maintaining worker processes.
The Worker processes do the actual processing of requests

# number of worker process
The number of worker processes is defined in the nginx.conf file and may be fixed for a given configuration or automatically adjusted to the number of available CPU cores
worker_processes

# ========================= Default  NGINX conf (/etc/nginx/nginx.conf)
All of the following are directives

# include directoeis
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;

# user nginx
user nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

# ==================== Contexts (Special Directives)
events, http, server, and location
user nobody; # a directive in the 'main' context

events {
  # configuration of events
}
http {
  # Configuration specific to HTTP and affecting all virtual servers
  server {
    # configuration of virtual server 1
    location /one {
      # configuration for processing URIs with '/one'
    }
    location /two {
      # configuration for processing URIs with '/two'
    }
  }
  server {
    # configuration of virtual server 2
  }
}

# =================== Reloading configs
# To reload your configuration, you can stop, or restart nginx, or send signals to the master process.
# A signal can be sent by running the NGINX executable with the -s parameter.
# stop — fast shutdown
# quit — graceful shutdown
# reopen — reopening the log files
# reload — reloading the configuration file

nginx -s signal


# ================ VARIABLES




###################################################################################################################################
# ############################################### MODULES ####################################################
#######################################################################################################################################
http://nginx.org/en/docs/http/ngx_http_upstream_module.html#directives
# ======================= ERROR PAGE
error_page   500 502 503 504  /50x.html;
location = /50x.html {
  root   /usr/share/nginx/html;
}

# ======================== RESTRICITNG ACCESS
location / {
  allow 192.168.1.1/24;
  allow 127.0.0.1;
  deny 192.168.1.2;
  deny all;
}

# ===================== AUTHENTICATION
# auth_basic directive
# we can customize it too
server {
  ...
  auth_basic "closed website";
  auth_basic_user_file conf/htpasswd;

  location /public/ {
    auth_basic off;
  }
}

# access if satisfy any condition
# if in the ip range or if not then authenticate
location / {
  satisfy any;

  allow 192.168.1.0/24;
  deny  all;

  auth_basic           "closed site";
  auth_basic_user_file conf/htpasswd;
}

# ======================= LIMITING ACCESS
limiting connections
limiting bandwidth
limiting zone

# ======================= BASIC CONFIGS
pid /path/to/buildout/var/nginx.pid;
lock_file /path/to/buildout/var/nginx.lock;
worker_processes 2;
daemon off;

# ====================== EVENTS
events {
  worker_connections 1024;
}


# ====================================================================== HTTP - UPSTREAM
    # The ngx_http_upstream_module module is used to define groups of servers that can be referenced by the
    # proxy_pass, fastcgi_pass, uwsgi_pass, scgi_pass, and memcached_pass directives.
    # It defines the other locations. Whereas proxy modules routes them to other locations.
    # It is where nginx routes the requests

    # ============== route to socket
    # To route to a socket file (write)
    upstream some_app_name {
      # Path to Unicorn SOCK file, as defined previously
      server unix:/tmp/some_path/some.sock fail_timeout=0;
    }

    error accessing sockets
    In layman terms, it appears that programs that create files in /tmp (or /var/tmp as I have discovered) are the only programs that are able to see the files in that directory. Unicorn was creating the UNIX socket file, however Nginx could not see it.


    # ============ Multiple routing and load balancing
    # route to multiple (load balancing). Weights can also be assigend. (if say my server 1 is powerful system)
    # Load balancing occurs in round robin fashion. THe server 1 is equivalent to 3 systems so 3 request will be given to it.
    # Queues are also present on the app server level too.
    # This means all requests for / go to the any of the servers listed under upstream XXX, with a preference for port 8000.
    # servers listening on ports and sockets can be mixed
    upstream myproject {
      server 127.0.0.1:8000 weight=3;
      server 127.0.0.1:8001;
      server 127.0.0.1:8002;
      server 127.0.0.1:8003;
    }

    # ========== More Opotions for serer
    weight # default 1
    max_fails # max fails that occur (default 1). Server is rendered unavilable then.
    fail_timeout # time to wait till the server does not respones (default 10sec). Server is renderered unavialble
    backup # marks server as backup. Only used when the primary servers are unavailable.
    down # marks server as permanentlly unavaialbe
    resolve
    max_conns
    slow_start
    max_conns

    # ========= Keepalive
    Activates the cache for connections to upstream servers.
    The connections to the upstream (outside servers) are kept alive in the cache
    SCGI and uwsgi protocols do not have a notion of keepalive connections.

    # ========= Healthchecks
    periduc health checks of the server groups

    # ========= Match

# ================== PASSING REQUEST TO PROXIED SERVERS
pass a request from NGINX to proxied servers over different protocols
modify client request headers that are sent to the proxied server
configure buffering of responses coming from the proxied servers
When NGINX proxies a request, it sends the request to a specified proxied server, fetches the response, and sends it back to the client.

# NGINX is an HTTP server (follows HTTP protocol)
# Servers for different application (python, php etc.) are app servers and are generally written in different protocl like CGI - FCGI, SCGI etc.

# NGINX can pass HTTP request to another HTTP server using proxy_pass
# the address may incldue a port too
location /some/path/ {
  proxy_pass http://www.example.com/link/;
}
location ~ \.php {
  proxy_pass http://127.0.0.1:8000;
}

fastcgi_pass passes a request to a FastCGI server
uwsgi_pass passes a request to a uwsgi server
scgi_pass passes a request to a SCGI server
memcached_pass passes a request to a memcached server

#
By default, NGINX redefines two header fields in proxied requests, “Host” and “Connection”
location /some/path/ {
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_pass http://localhost:8000;
}




# ==================================== COMPRESSION AND DECOMPRESSION
# gzip on/off
gzip  on;
# version of http
gzip_http_version 1.0;
gzip_comp_level 2;
gzip_proxied any;
gzip_vary off;
# types to gzip
gzip_types text/plain text/css application/x-javascript text/xml application/xml application/rss+xml application/atom+xml text/javascript application/javascript application/json text/mathml;
# min length only then gzip
gzip_min_length  1000;
gzip_disable     "MSIE [1-6]\.";
# To send a compressed version of a file to the client instead of the regular one, set the gzip_static directive to on within the appropriate context.
# nginx will try to find file.gz and serve that
location / {
  gzip_static on;
}

# ===================================================================== HTTP - SERVER
# It is used to create a virtual server
# It is where the nginx captures requests
# If we put loacalhost externally it gets resolved to the IP (eg. 54.254.144.150:443)
# So it will make connections to the above IP
# But if we have a Domain name (getamber.in) mapped to our Server IP we can
# restrict connections to IP but allow www.domain.com using the following

server {
  listen 80;
  server_name www.domain.com;
  location / {
    proxy_pass http://myproject;
  }
}

#=========== Listen and server_name
This directive specifies the IP address and port (or Unix domain socket and path) which the server will listen.
The IP address can be either IPv4 or IPv6. Keep in mind that IPv6 must be specified in the square brackets.
If a port is omitted, the standard port will be used.
Likewise, if an address is omitted, the server will listen on all addresses.
server {
  listen 127.0.0.1:8080;
  # The rest of server configuration
}
server {
  listen 80;
  # The rest of server configuration
}
There can be multiple servers to be listened. We can get to the specific by proviing the server_name
server {
  listen      80;
  server_name example.org www.example.org;
}

# ========= Using Root
serving files directly (static content)(no upstream)
to the request with the URI /images/some/path/ NGINX will respond with /www/data/images/some/path/index.html
Here we have defined the root
So /images/ will map to /www/data/images/index.html
A request with a URI such as /any/path/file.mp3 will be mapped to /www/media/any/path/file.mp3

server {
  root /www/data;

  location / {
  }

  location /images/ {
  }

  location ~ \.(mp3|mp4) {
    root /www/media;
  }
}

# ========= AutoIndex
If this file does not exist, a 404 error will be returned by default.
It is possible, however, to return an automatically generated directory listing when the index file does not exist by setting the autoindex directive to on.
If index.html is not present
location /images/ {
  autoindex on;
}

# =========== Headers

# ============ Try Files


# ===================== Location module
# Location works like routes
  location 1{

  }
  location 2{
    //backend
  }
# if something does not exist in location 1 then location 2 is executed
# Suppose we serve static files from location 1 and if no files are there we pass it to backend
# Nginx can proxy requests using http, FastCGI, uwsgi, SCGI, or memcached.
FastCGI proxyin
# https://www.digitalocean.com/community/tutorials/understanding-and-implementing-fastcgi-proxying-in-nginx

proxy_set_header                Host $host;
proxy_set_header                X-Real-IP $remote_addr;
proxy_set_header                X-Forwarded-For $proxy_add_x_forwarded_for;
client_max_body_size            0;
client_body_buffer_size         128k;
proxy_send_timeout              120;
proxy_buffer_size               4k;
proxy_buffers                   4 32k;
proxy_busy_buffers_size         64k;
proxy_temp_file_write_size      64k;
proxy_connect_timeout           75;
proxy_read_timeout              205;

# =============== CGI Modules
CGI is the link between the web server and the application. CGI are of various types.
The application server implements the various protocols for CGI.
# HttpFastcgiModule
HttpFastcgiModule is for when you want nginx to talk to a fastcgi server.

# ============== HttpProxyModule
HttpProxyModule is for when you want nginx to talk to a http (or https) server.
Unicorn is an HTTP server so proxy module is required


# ============ multiple servers can also be spcified
upstream images_app_server {
  server unix:/srv/dev/housing.images/tmp/sockets/unicorn.sock fail_timeout=0;
}

server {
  listen       80 ;
  server_name  staging.images.housing.com images.staging.housing.com;

  location / {
    proxy_pass http://images_app_server;
    proxy_set_header  Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

}

# ================ use case of multiple in case of backup servers
upstream backend {
  server backend1.example.com       weight=5;
  server backend2.example.com:8080;
  server unix:/tmp/backend3;

  server backup1.example.com:8080   backup;
  server backup2.example.com:8080   backup;
}

server {
  location / {
    proxy_pass http://backend;
  }
}
# ====================== Port Permissions
# Unicorn listes to ports (by default 8008)
# Nginx can put the ports in upstream to write to
# There can be permission denied when connecting to upstream
# This can be because SElinux allows outbind connections only on some specific ports
http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000
# To fix the problem, you simply need to add your own desired port number to the list.
semanage port --add --type http_port_t --proto tcp 7777
# Then you will see the port number added into the list, and your connections should then work.
# semanage port --list
http_port_t                    tcp      7777, 80, 81, 443, 488, 8008, 8009, 8443, 9000

# SerIt seemed to be a SELinux related problem. As suggested in this question I have tried using
setsebool -P httpd_can_network_connect 1

# I forgot what turns out to be a very important detail to this configuration. It's on a Fedora server running selinux. Port 8080 was out of context in nginx and being denied.
type=AVC msg=audit(1380053745.510:1730): avc:  denied  { name_connect } for  
   pid=12145 comm="nginx" dest=8080 scontext=system_u:system_r:**httpd_t**:s0
   tcontext=system_u:object_r:**http_cache_port_t**:s0 tclass=tcp_socket

http_cache_port_t              tcp      8080, 8118, 8123, 10001-10010
http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000



I had a similar issue getting Fedora 20, Nginx, Node.js, and Ghost (blog) to work. It turns out my issue was due to SELinux.

I checked for errors in the SELinux logs:

sudo cat /var/log/audit/audit.log | grep nginx | grep denied
And found that running the following commands fixed my issue:

sudo cat /var/log/audit/audit.log | grep nginx | grep denied | audit2allow -M mynginx
sudo semodule -i mynginx.pp

References:

http://blog.frag-gustav.de/2013/07/21/nginx-selinux-me-mad/ https://wiki.gentoo.org/wiki/SELinux/Tutorials/Where_to_find_SELinux_permission_denial_details http://wiki.gentoo.org/wiki/SELinux/Tutorials/Managing_network_port_labels http://www.linuxproblems.org/wiki/Selinux



53
down vote
I’ve run into this problem too. Another solution is to toggle the SELinux boolean value for httpd network connect to on (Nginx uses the httpd label).

setsebool httpd_can_network_connect on
To make the change persist use the -P flag.

setsebool httpd_can_network_connect on -P
You can see a list of all available SELinux booleans for httpd using

getsebool -a | grep httpd





# ==================== reqwrite
A request URI can be modified multiple times during request processing through the use of the rewrite. This directive has two required and one optional parameters. The first parameter is a regular expression to test the request URI. The second parameter is the URI to replace the current one if it matches the regular expression.
The optional third parameter is a flag that can indicate to stop processing further rewrite directives.
server {
  ...
  rewrite ^(/download/.*)/media/(.*)\..*$ $1/mp3/$2.mp3 last;
  rewrite ^(/download/.*)/audio/(.*)\..*$ $1/mp3/$2.ra  last;
  return  403;
  ...
}

# ==================== NGINX variablrs
The nginx.conf file also allow you to set variables. Variables help you create configurations that behave differently depending on defined circumstances. Variables are named values that are calculated at run time and are used in parameters of directives. A variable is denoted by the “$” sign at the beginning of its name. Variables define information based upon nginx’s state, such as the properties of the currently processed request.

There are plenty of predefined variables, such as the core HTTP variables, but you can also define custom variables using the set, map, and geo directives. Most variables are computed at run time and contain information related to a specific request. For example, $remote_addr contains the client IP address and $uri holds the current URI value.


# ======================= try files
The try_files directive can be used to check whether the specified file or directory exists and make an internal redirect, or return a specific status code if they don’t. For example, to check the existence of a file corresponding to the request URI, use the try_files directive and the $uri variable as follows:
server {
  root /www/data;

  location /images/ {
    try_files $uri /images/default.gif;
  }
}
The file is specified in the form of the URI, which is processed using the root or alias directives set in the context of the current location or virtual server. In this case, if the file corresponding to the original URI doesn’t exist NGINX makes an internal redirect to the URI specified in the last parameter returning /www/data/images/default.gif. The last parameter can also be a status code (specified after =) or the name of a location. In the following example, a 404 error is returned if none of the options resolves into an existing file or directory.


# ===================== Proxy Caching
caching the respones that are obtained from the proxy servers

http {
  ...
  # defined at the top lovel
  # path to where proxy will be stored
  # the key/zone for the cache. Max size param also present
  proxy_cache_path /data/nginx/cache keys_zone=one:10m;

  server {
    proxy_cache one;
    location / {
      proxy_pass http://localhost:8000;
    }
  }
}

http {
  ...
  proxy_cache_path /data/nginx/cache keys_zone=one:10m
  loader_threshold=300 loader_files=200
  max_size=200m;

  server {
    listen 8080;
    proxy_cache one;

    location / {
      proxy_pass http://backend1;
    }

    location /some/path {
      proxy_cache_valid any   1m;
      proxy_cache_min_uses 3;
      proxy_cache_bypass $cookie_nocache $arg_nocache$arg_comment;
      proxy_pass http://backend2;
    }
  }
}


This example defines a virtual server with two locations that use the same cache but with different settings. It is assumed that responses from the backend1 server rarely change and can be cached the first time a request is received and held for as long as possible. By contrast, responses from the backend2 server are highly volatile, and therefore are cached only after three occurrences of the same request and held for one minute. Moreover, if a request satisfies the conditions of the proxy_cache_bypass directive, the cache will not be searched for the response at all and NGINX will immediately pass the request to the backend.



###################################################################################################################################
# ############################################### LOGS ####################################################
#######################################################################################################################################
NGINX writes information about encountered issues of different severity levels to the error log.
The error_log directive sets up logging to a partifular file, stderr, or syslog and specifies the minimal severity level of messages to log.
By default errors are logged to var/logs/nginx/errors.log

# ============= error logs
# changing severity of logs from error to warn
# this can be added anywhere in the config files
# These settings work globally
# The settings in the main context is inherited by all others
# These can also be speified in the server for server specific error_log configs
error_log logs/error.log warn;
warn, error, crit, alert, and emerg levels will be logged.

# =========== access logs
acess log written whenever client makes a connection
access_logs directive is present to define the location
log_format can also be defined
http {
  log_format compression '$remote_addr - $remote_user [$time_local] '
  '"$request" $status $body_bytes_sent '
  '"$http_referer" "$http_user_agent" "$gzip_ratio"';

  server {
    gzip on;
    access_log /spool/logs/nginx-access.log compression;
    ...
  }
}

# =========== conditional logging
if we do not want to log 200 and 300
map $status $loggable {
  ~^[23]  0;
  default 1;
}
access_log /path/to/access.log combined if=$loggable;


# ========= Syslog logging
Syslog is a standard for computer message logging and allows collecting log messages from different devices on a single syslog server. In NGINX, logging to syslog is configured with the syslog: prefix in error_log and access_log directives.

/var/logs/nginx
Setting up the error log
Setting up the access log
Logging to syslog
Live activity monitoring
http://nginx.com/resources/admin-guide/logging-and-monitoring/





###################################################################################################################################
# ############################################### DIRECTIVES ####################################################
#######################################################################################################################################
NGINX has several directive built into it
Each directive that is included is responseible for certain functionality
Like there is cache directive, log directive etc





###################################################################################################################################
# ############################################### CONFIGURATIONS ####################################################
#######################################################################################################################################
# ======================= Firewall
firewall applies all the riles refardin what ports are opened what are not


# check if nginx is listening
netstat -aln | grep 80



# ======================== FLUSH IP TABLE ROOLS
# list rules
iptables -L
# ip table flushing
iptables -F
http://serverfault.com/questions/464292/can-not-access-ec2-server-via-ip-address

Things to check:

Your elastic IP associated with your instance?
Your security group of instance permits incoming connections?
Your instance firewall permits incoming connections?
Your application listens?

# ================================== ADVANCED CONFIGURATION FOR CACHING
Nginx has a built in caching system
Usually with users with login we want to cache only the outer pages
# do not cache when users are logged in..
proxy_cache_bypass $cookie___ac;




# ================================= NGINX SAMPLE CONFIGS
server {
  listen       80 ;
  server_name  acl.staging.housing.com;

  location / {
    proxy_pass http://127.0.0.1:4007;
    proxy_set_header  Host $host;
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

}


# allow and deny all
# reverse proxy for routing the 80 port internally to 4007
# this is port routing instead of socket
# The
server {
  listen       80 ;
  server_name  acl.internal.staging.housing.com;

  allow 10.1.1.219/32;
  deny all;

  location / {
    proxy_pass http://127.0.0.1:4007;
    proxy_set_header  Host $host;
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

}


# ========================== SOCKET CONFIGURATION
upstream app {
  # Path to Unicorn SOCK file, as defined previously
  server unix:/srv/www/unicorn.amber.frontend.sock fail_timeout=0;
}

# server name needs to be changed to the
server {
  listen 80;
  server_name localhost;

  # Application root, as defined previously
  root /srv/www/amber.frontend/current/public;

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

# ===================== SSL Configuration
server {
  listen       443 ssl;
  server_name  *.staging.housing.com staging.housing.com;

  ssl_certificate      /etc/nginx/housing-ssl/server.crt;
  ssl_certificate_key  /etc/nginx/housing-ssl/server.key;

  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout  5m;

  ssl_ciphers  HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers   on;

  #	add_header Strict-Transport-Security "max-age=31536000; includeSubdomains";

  location / {
    proxy_pass http://127.0.0.1:80;
    proxy_set_header  Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

  }
}

server {
  listen       443 ssl;
  server_name  *.staging.housingcdn.com;

  ssl_certificate      /etc/nginx/housingcdn-ssl/server.crt;
  ssl_certificate_key  /etc/nginx/housingcdn-ssl/server.key;

  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout  5m;

  ssl_ciphers  HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers   on;

  #	add_header Strict-Transport-Security "max-age=31536000; includeSubdomains";

  location / {
    proxy_pass http://127.0.0.1:80;
    proxy_set_header  Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}

upstream login_app_server {
  server unix:/srv/dev/housing.clients/tmp/sockets/unicorn.sock fail_timeout=0;
}

# ===================== ANOTHER CONFIG
# all the virtual servers have been scoped

server {
  listen       80 ;
  server_name  login.staging.housing.com;

  root /srv/dev/housing.clients/public;

  try_files $uri/index.html $uri.html $uri @app;

  location @app {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $host;
    proxy_set_header  Client-IP "";
    proxy_pass http://login_app_server;
  }

  error_page 500 502 503 504 /500.html;

  error_page 503 @maintainence;

  location @maintainence {
    rewrite ^(.*)$ /maintainence.html break;
  }

}

server {
  listen       80 ;
  server_name  login.internal.staging.housing.com;

  root /srv/dev/housing.clients/public;

  try_files $uri/index.html $uri.html $uri @app;

  location @app {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $host;
    proxy_set_header  Client-IP "";
    proxy_pass http://login_app_server;
  }

  error_page 500 502 503 504 /500.html;

  error_page 503 @maintainence;

  location @maintainence {
    rewrite ^(.*)$ /maintainence.html break;
  }

}
