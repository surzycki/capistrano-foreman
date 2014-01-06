# Capistrano::Foreman

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


**Export Procfile to upstart**

This task will be run before `deploy:restart` as part of Capistrano's default deploy, or can be run in isolation with:

    cap production foreman:export

**NOTE** 

In order for foreman to export to upstart your deploy user must have `sudoer` privileges 

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
