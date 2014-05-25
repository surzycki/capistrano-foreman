# Capistrano::Foreman

[![Gem Version](https://badge.fury.io/rb/capistrano3-foreman.png)](http://badge.fury.io/rb/capistrano3-foreman)

Capistrano V3 for Foreman - to generate and use Upstart scripts.

## WIP

There be dragons, proceed at your own risk.

## Installation

Add this line to your application's Gemfile:
```ruby
gem 'capistrano3-foreman'
```
And then execute:
```bash
$ bundle
```
Or install it yourself as:
```bash
$ gem install capistrano3-foreman
```
## Usage

Require in Capfile to use the default task:
```ruby
require 'capistrano/foreman'
```

###Export Procfile to upstart###

This task will be run before `deploy:restart` as part of Capistrano's default deploy, or can be run in isolation with:
```bash
cap production foreman:export
```

**NOTE**

In order for foreman to export to upstart your deploy (Capistrano) user must have `sudoer` privileges


### Configuration

Configurable options; shown here with defaults:

```ruby
set :foreman_roles, %w(app) # roles to run capistrano-foreman on
set :foreman_application, -> { fetch(:application) } # name to use when exporting/starting/stopping Foreman services
set :foreman_user, -> nil # username to use when generating services; this defaults to the user that Cap's using for SSH.
set :foreman_log_path, -> { shared_path.join('log') } # log path, defaults to your deployment's /path/to/cap/shared/log
set :foreman_env, nil # .env file for foreman to inject into the exported service.
```

For more details on the custom ENVIRONMENT variables for Foreman [(see here)](http://ddollar.github.io/foreman/#ENVIRONMENT).


## The Twelve Factor App

[(Treat backing services as attached resources)](http://12factor.net/backing-services) by using ENV variables for your configuration.

**database.yml**
```yaml
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
```

**deploy.rb**
```ruby
set :foreman_env,     '/home/deploy/.pam_environment'
```

**.pam_environment**
```bash
DATABASE_HOST=database.example.com
DATABASE_USERNAME=user
DATABASE_PASSWORD=password
RAILS_ENV=staging
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
