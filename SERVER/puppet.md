

<!-- Resoure Types -->
Resource Types are single units of configuration composed by:

A type (package, service, file, user, mount, exec ...)

A title (how is called and referred)

Zero or more arguments

type { 'title':
  argument  => value,
  other_arg => value,
}
Example for a file resource type:

file { 'motd':
  path    => '/etc/motd',
  content => 'Tomorrow is another day',
}


# To get the description of resources use the following command
From the shell the command line interface:
puppet describe file
For the full list of available descriptions try:
puppet describe --list



# Installation of OpenSSH package
package { 'openssh':
  ensure => present,
}
Creation of /etc/motd file

file { 'motd':
  path => '/etc/motd',
}
Start of httpd service

service { 'httpd':
  ensure => running,
  enable => true,
}





# Management of nginx service with parameters defined in module's variables
service { 'nginx':
  ensure     => $::nginx::manage_service_ensure,
  name       => $::nginx::service_name,
  enable     => $::nginx::manage_service_enable,
}
Creation of nginx.conf with content retrieved from different sources (first found is served)

file { 'nginx.conf':
  ensure  => present,
  path    => '/etc/nginx/nginx.conf',
  source  => [
      "puppet:///modules/site/nginx.conf--${::fqdn}",
      "puppet:///modules/site/nginx.conf" ],
}


Use puppet resource to interrogate the RAL:

puppet resource user

puppet resource user root

puppet resource package

puppet resource service
Or to directly modify resources:

puppet resource service httpd ensure=running enable=true