Mina deploy script is just a ruby script that requires the Mina gem
it runs outside the rails environment just like a standalong ruby script
It can reqad all the environment variables of the system using ENV

https://www.infinum.co/the-capsized-eight/articles/faster-web-application-deployments-using-mina-instead-of-capistrano

# =========================================== DEPLOYMENTS
Repo is pulled
migrations are run
assets are compiled
after all of this unicorn is restarted
unicorn loads all the application in the memory
if the unicorn takes long to restart the site is down for that time
this causes downtime. Symlinking cannot prevent this.

queue! "cd /home/username/webapps/appname"
queue! "export GEM_HOME=$PWD/gems"
queue! "export RUBYLIB=$PWD/lib"
queue! "export PATH=$PWD/bin:$PATH"
queue! "export LD_LIBRARY_PATH=$HOME/lib/"

# set :version, "v2.0.5"

# settings.version    #=> "v2.0.5"
# settings.version?   #=> true


# task :restart do
# queue "#{settings.nginx_path!}/sbin/nginx restart"
# end

# $ mina restart
# Error: You must set the :nginx_path setting

# set :admin_email, "johnsmith@gmail.com"

# task :email do
# set :message, "Deploy is done"

# system "echo #{message} | mail #{admin_email}"
# end

# How Zero downtime deploy works with symlinking
# The current version keeps on running
# We build our app in tmp, copy it to release and symlink that to current


set_default :unicorn_role,      -> { user }
set_default :unicorn_env,       -> { fetch(:rails_env, 'production') }
set_default :unicorn_config,    -> { "#{deploy_to}/#{current_path}/config/unicorn.rb" }
set_default :unicorn_pid,       -> { "#{deploy_to}/#{current_path}/tmp/pids/unicorn.pid"  }
set_default :unicorn_cmd,       -> { "#{bundle_prefix} unicorn" }
set_default :unicorn_restart_sleep_time, -> { 2 }
set_default :bundle_gemfile,    -> { "#{deploy_to}/#{current_path}/Gemfile" }


desc "Set the environment variables."
task :set_env_variables => :environment do
  queue! "cd /home/username/webapps/appname"
  queue! "export GEM_HOME=$PWD/gems"
  queue! "export RUBYLIB=$PWD/lib"
  queue! "export PATH=$PWD/bin:$PATH"
  queue! "export LD_LIBRARY_PATH=$HOME/lib/"
end

# ========================================= VARIABLES
# The deploy scriprs should go in version control as they may be used to deploy in staging prduction etc.
# By example, I can specifiy a branch and a domain to mina when I deploy with this command :
# $ mina deploy DOMAIN=1.2.3.4 BRANCH=my_specific_branch

# With in my deploy.rb :

# set :user, 'my_app'
# set :domain, ENV['DOMAIN']
# set :identity_file, ENV['IDENTITY_FILE']

# set :deploy_to, '/home/app'
# set :repository, 'git@github.com:foo/bar.git'
# set :branch, ENV['BRANCH'] || 'master'
# mina is essentially rake and it is possible to simply:

# rake mytask var=foo
# p ENV['var'] # => "foo"
# Rails.root

# set environment varialbe to RAILS_ENV = production
# that way we will not need to specify   RAILS_ENV=production rake db:migrate
# mina init (initalize deploy.rb)
# mina setup --verbose
# mina deploy --trace
# maintenance environment for apps


# =============================================



-----> Creating the build path
$ mkdir tmp/build-128293482394
-----> Cloning the Git repository
$ git clone https://github.com/nadarei/flipstack.git . -n --recursive
Cloning... done.
-----> Installing gem dependencies using Bundler
$ bundle install --without development:test
Using i18n (0.6.0)
Using multi_json (1.0.4)
...
Your bundle is complete! It was installed to ./vendor/bundle
-----> Moving to releases/4
$ mv "./tmp/build-128293482394" "releases/4"
-----> Symlinking to current
$ ln -nfs releases/4 current
-----> Launching
$ cd releases/4
$ sudo service nginx restart
-----> Done. Deployed v4


require 'mina/bundler'
require 'mina/rails'
require 'mina/git'
require 'mina/rvm'

# Usually mina focuses on deploying to one host and the deploy options are therefore simple.
# In our case, there is a number of possible servers to deploy to, it is therefore necessary to
# specify the host that we are targeting.
server = ENV['server']
# Since the same host can have multiple applications running in parallel, it is necessary to
# specify further which application we want to deploy
version = ENV['version']

# Set the repository (here on BitBucket)
set :repository, 'git@bitbucket.org:username/project.git'
# setting the term_mode to system disable the "pretty-print" but prevent some other issues
set :term_mode, :system

# Manually create these paths in shared/ (eg: shared/config/database.yml) in your server.
# They will be linked in the 'deploy:link_shared_paths' step.
set :shared_paths, ['config/database.yml', 'log']

# Optional SSH settings:
# SSH forward agent to ensure that credentials are passed through for git operations
set :forward_agent, true

##########################################################################

# Setup environment

##########################################################################

# This task is the environment that is loaded for most commands, such as
# `mina deploy` or `mina rake`.
task :environment do
# Ensure that a server has been set
  unless server
    print_error "A server needs to be specified."
    exit
  end

# Remote application folder
  set :deploy_to, "/home/username/project/#{version}"

# Set the basic environment variables based on the server and version
  case server
  when 'qa'
    # The hostname to SSH to
    set :domain, 'qa-domain.com'
    # SSH Optional settings
    set :user, 'foobar'    # Username in the server to SSH to.
    # set :port, '30000'     # SSH port number.
    # Rails environment
    set :rails_env, 'qa'
  when 'prod'
    # The hostname to SSH to
    set :domain, 'prod-domain.com'
    # SSH Optional settings
    set :user, 'foobar'    # Username in the server to SSH to.
    # set :port, '30000'     # SSH port number.
    # Rails environment
    set :rails_env, 'production'
  end

# For those using RVM, use this to load an RVM version@gemset.
  invoke :'rvm:use[ruby-1.9.3-p327@project]'
end

##########################################################################

# Create new host tasks
# Tasks below are related to deploying a new version of the application

##########################################################################

# Function extracted from http://blog.nicolai86.eu/posts/2013-05-06/syncing-database-content-down-with-mina
# allowing to read the content of the database.yml file
RYAML = <<-BASH
function ryaml {
  ruby -ryaml -e 'puts ARGV[1..-1].inject(YAML.load(File.read(ARGV[0]))) {|acc, key| acc[key] }' "$@"
  };
  BASH

# Execute all setup tasks defined below
  desc "Create new folder structure + database.yml + DB + VirtualHost"
  task :'setup:all' => :environment do
    queue! %[echo "-----> Setup folder structure on server"]
    invoke :setup
    queue! %[echo "-----> Setup the DB (create user / DB)"]
    invoke :'setup:db'
    queue! %[echo "-----> Setup Apache VirtualHost Configuration"]
    invoke :'setup:apache'
    queue! %[echo "-----> Deploy Master for this version"]
    invoke :deploy
    queue! %[echo "-----> Enable Apache host and restart Apache"]
    invoke :'apache:enable'
  end

# Put any custom mkdir's in here for when `mina setup` is ran.
# For Rails apps, we'll make some of the shared paths that are shared between
# all releases.
  task :setup => :environment do
    queue! %[mkdir -p "#{deploy_to}/shared/log"]
    queue! %[chmod g+rx+ +u+rwx "#{deploy_to}/shared/log"]

    queue! %[mkdir -p "#{deploy_to}/shared/config"]
    queue! %[chmod g+rx,u+rwx "#{deploy_to}/shared/config"]

    queue! %[touch "#{deploy_to}/shared/config/database.yml"]
    queue  %[echo "-----> Fill in information below to populate 'shared/config/database.yml'."]
    invoke :'setup:db:database_yml'
  end

# Populate file database.yml with the appropriate rails_env
# Database name and user name are based on convention
# Password is defined by the user during setup
  desc "Populate database.yml"
  task :'setup:db:database_yml' => :environment do
    puts "Enter a name for the new database"
    db_name = STDIN.gets.chomp
    puts "Enter a user for the new database"
    db_username = STDIN.gets.chomp
    puts "Enter a password for the new database"
    db_pass = STDIN.gets.chomp
    # Virtual Host configuration file
    database_yml = <<-DATABASE.dedent
    #{rails_env}:
    adapter: mysql2
    encoding: utf8
    database: #{db_name}
    username: #{db_username}
    password: #{db_pass}
    host: localhost
    timeout: 5000
    DATABASE
    queue! %{
      echo "-----> Populating database.yml"
      echo "#{database_yml}" > #{deploy_to!}/shared/config/database.yml
      echo "-----> Done"
    }
  end

# Create the new database based on information from database.yml
# In this application DB, user is given full access to the new DB
  desc "Create new database"
  task :'setup:db' => :environment do
    queue! %{
      echo "-----> Import RYAML function"
      #{RYAML}
      echo "-----> Read database.yml"
      USERNAME=$(ryaml #{deploy_to!}/#{shared_path!}/config/database.yml #{rails_env} username)
      PASSWORD=$(ryaml #{deploy_to!}/#{shared_path!}/config/database.yml #{rails_env} password)
      DATABASE=$(ryaml #{deploy_to!}/#{shared_path!}/config/database.yml #{rails_env} database)
      echo "-----> Create SQL query"
      Q1="CREATE DATABASE IF NOT EXISTS $DATABASE;"
      Q2="GRANT USAGE ON *.* TO $USERNAME@localhost IDENTIFIED BY '$PASSWORD';"
      Q3="GRANT ALL PRIVILEGES ON $DATABASE.* TO $USERNAME@localhost;"
      Q4="FLUSH PRIVILEGES;"
      SQL="${Q1}${Q2}${Q3}${Q4}"
      echo "-----> Execute SQL query to create DB and user"
      echo "-----> Enter MySQL root password on prompt below"
      #{echo_cmd %[mysql -uroot -p -e "$SQL"]}
      echo "-----> Done"
    }
  end

# Create a new VirtualHost file
# Server name is defined by convention
# Script executes some sudo operations
  desc "Create Apache site file"
  task :'setup:apache' => :environment do
    # Get variable for virtual host configuration file
    fqdn = get_fqdn(server, version)
    fqdn_ext = external_fqdn(server, version)
    # Virtual Host configuration file
    vhost = <<-HOSTFILE.dedent
    <VirtualHost *:80>
    ServerAdmin user@your-website.com
    ServerName #{get_fqdn(server, version)}
    DocumentRoot #{deploy_to!}/#{current_path!}/public
    RailsEnv #{rails_env}
    <Directory #{deploy_to!}/#{current_path!}/public>
    Options -MultiViews
    AllowOverride all
    </Directory>
    PassengerMinInstances 5
    # Maintenance page
    ErrorDocument 503 /503.html
    RewriteEngine On
    RewriteCond %{REQUEST_URI} !.(css|gif|jpg|png)$
    RewriteCond %{DOCUMENT_ROOT}/503.html -f
    RewriteCond %{SCRIPT_FILENAME} !503.html
    RewriteRule ^.*$ - [redirect=503,last]
    </VirtualHost>
    HOSTFILE
    queue! %{
      echo "-----> Create Temporary Apache Virtual Host"
      echo "#{vhost}" > #{fqdn}.tmp
      echo "-----> Copy Virtual Host file to /etc/apache2/sites-available/ (requires sudo)"
      #{echo_cmd %[sudo cp #{fqdn}.tmp /etc/apache2/sites-available/#{fqdn}]}
      echo "-----> Remove Temporary Apache Virtual Host"
      rm #{fqdn}.tmp
      echo "-----> Done"
    }
  end

# Enable the new Virtual Host and restart Apache
  desc "Enable new Apache host file"
  task :'apache:enable' => :environment do
    fqdn = get_fqdn(server, version)
    queue! %{
      echo "-----> Enable Apache Virtual Host"
      #{echo_cmd %[sudo a2ensite #{fqdn}]}
      echo "-----> Remove Temporary Apache Virtual Host"
      #{echo_cmd %[sudo service apache2 reload]}
    }
  end

  ##########################################################################

# Deployment related task

  ##########################################################################

  desc "Deploys the current version to the server."
  task :deploy => :environment do
    deploy do
      # Put things that will set up an empty directory into a fully set-up
      # instance of your project.
      invoke :'git:clone'
      invoke :'deploy:link_shared_paths'
      invoke :'bundle:install'
      invoke :'rails:db_migrate'
      invoke :'rails:assets_precompile:force'

      to :launch do
        queue "touch #{deploy_to}/#{current_path}/tmp/restart.txt"
      end
    end
  end

  #########################################################################

# Helper functions

  ##########################################################################


# Get the main domain based on the server

# @return [String] the main domain
  def main_domain(server)
    case server
    when 'qa'
      "qa-domain.com"
    when 'prod'
      "prod-domain.com"
    end
  end


# Fully Qualified Domain Name of the host
# Concatenation of the version and the domain name

# @return [String] the FQDN
  def get_fqdn(server, version)
    fqdn = "#{version}.#{main_domain(server)}"
    return fqdn
  end

  #########################################################################

# Libraries

  ##########################################################################


# See https://github.com/cespare/ruby-dedent/blob/master/lib/dedent.rb

  class String
    def dedent
      lines = split "\n"
      return self if lines.empty?
      indents = lines.map do |line|
        line =~ /\S/ ? (line.start_with?(" ") ? line.match(/^ +/).offset(0)[1] : 0) : nil
      end
      min_indent = indents.compact.min
      return self if min_indent.zero?
      lines.map { |line| line =~ /\S/ ? line.gsub(/^ {#{min_indent}}/, "") : line }.join "\n"
    end
  end