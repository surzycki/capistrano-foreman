# Capistrano::Foreman

[![Gem Version](https://badge.fury.io/rb/capistrano3-foreman.png)](http://badge.fury.io/rb/capistrano3-foreman)

Capistrano V3 for foreman

## WIP

There be dragons, proceed at your own risk.

## Installation

Add this line to your application's Gemfile:

    gem 'capistrano3-foreman'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install capistrano3-foreman

## Usage

Require in Capfile to use the default task:

    require 'capistrano/foreman'


###Export Procfile to upstart###

This task will be run before `deploy:restart` as part of Capistrano's default deploy, or can be run in isolation with:

    cap production foreman:export

**NOTE** 

In order for foreman to export to upstart your deploy user must have `sudoer` privileges 


###Options###

Custom ENVIRONMENT variables for foreman [(see here)](http://ddollar.github.io/foreman/#ENVIRONMENT). 

    set :foreman_env,  '/remote/path/to/your.env'         # Default none 

Custom location for the target upstart configuration files (defaults to `/etc/init`):

    set :foreman_location, '/etc/init/my-application'

Use other user than `root` (the default) to run `bundle exec foreman export` when the `foreman:export` task is invoked:

    set :foreman_export_user, 'my_user'

Specify the name of the service using `foreman_service_name`:

    set :foreman_service_name, 'my/application'


##The Twelve Factor App##

[(Treat backing services as attached resources)](http://12factor.net/backing-services) by using ENV variables for your configuration.

**database.yml**

    default: &default
      adapter: mysql2
      host: <%= ENV['DATABASE_HOST'] %>
      username: <%= ENV['DATABASE_USERNAME'] %>
      password: <%= ENV['DATABASE_PASSWORD'] %>
      encoding: utf8
      reconnect: true
    
    staging:
      <<: *default
      database: app_web_staging
    
    production:
      <<: *default
      database: app_web_production

**deploy.rb**

    set :foreman_env,     '/home/deploy/.pam_environment'

**.pam_environment**

    DATABASE_HOST=database.example.com
    DATABASE_USERNAME=user
    DATABASE_PASSWORD=password
    RAILS_ENV=staging

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
