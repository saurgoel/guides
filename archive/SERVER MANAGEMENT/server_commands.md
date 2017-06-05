https://serversforhackers.com/hosting-web-applications

Hosting a web application requires the orchestration of three actors:

The Application
The Gateway
The Web Server

Applications & HTTP Interfaces

# Web Applications are generally coded using a framework or suite of libraries. These typically have tooling to handle HTTP requests.
#
# Libraries created to accept and translate HTTP requests are referred to as HTTP Interfaces. These accept requests and translate them for application code.
#
# For example, Python has the WSGI specification. This specifies an interface between web servers and Python applications.
#
# A popular implementation of WSGI is Werkzeug. This is a Python library that can accept and parse WSGI-compliant web requests.
#
# Similarly, Ruby has Rack. Rack is a specification and library that can accept Rack-compliant web requests.
#
# Most languages have HTTP interfaces available. Python and Ruby have HTTP interfaces added on via specifications and libraries. However, many newer languages include HTTP interfaces as part of their standard library.
#
# NodeJS and Golang are two examples of languages that can listen for HTTP requests "out of the box". HTTP requests can be given and parsed without needing libraries or specifications.
#
# PHP, notably, is lacking such a specification. However, there are talks of PHP-FIG defining such an interface in PSR-7.
# In any case, web applications must incorporate a way to accept web requests and return valid responses.
#
#   We've discussed how languages specify how to accept web requests. Next, we'll discuss how application gateways translate HTTP requests.
#
#   The application request flow and its interaction with a gateway is pictured here.



httpd server


# The task that the main server should do should be basic serving of web pages.
# Other tasks that require mails to be sent, third party apis to be invoked, etc. must be moved to workers or background processing
# The server must not be kept waiiting for such type of things because server is used for realtime.

# Performance enhancement
# We can run multiple instances of servers. Similary we can also run multiple instances of background workers
# difference between servers and workers in their configuration. Optimization required in each.
# There is a request queue maintetaied for each server. Once the server is processing one request the other is kept waiting.
# In the controller commands decide what to do .. realtime process or put in worker

# Mac connected to wiki, ip of mac on wifi network - 10.1.22.106 (dynamic/static) the ip assigned. how ip is assigned.
# accessing thorugh vm ip - 10.1.10.4
