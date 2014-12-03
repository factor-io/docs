---
title: Migrating from Factor.io Cloud
category: Guides
---

# Step 1: Install Prereqs

## Install Ruby 2.1.3
There are  a number of ways to install Ruby, but we recommend using [RVM](https://rvm.io/). Follow their install instructions to get started.

First, follow the [RVM install instructions](https://rvm.io/rvm/install).

Next, Use RVM to install and run Ruby 2.1.3

```shell
rvm install ruby-2.1.3
rvm use ruby-2.1.3 --default
```

## Install the Factor.io Runtime

```shell
gem install factor
```

# Step 2: Create your working directory

```shell
mkdir ~/workflows
cd ~/workflows
```

# Step 3: Create your new workflows

First run this command to get list of available Jumpstarts.

```shell
factor registry workflows list
```

Using the ID provided in the list (the value in paranthesis) run this command

```shell
factor registry workflows add github-to-heroku
```

This will prompt you for credentials and other settings.

Once this is done, you will have a new `.rb` file, `connectors.yml`, and `credentials.yml` in your file.

# Step 4: Deploy workflow 
We are going to deploy this application to a dedicated server, but to do that, we need to make sure all the prereqs come with it.

Lets create a gem file `Gemfile`:

```ruby
source "https://rubygems.org"

gem 'foreman'
gem 'factor'
```

Now lets install those dependencies and create the `Gemfile.lock` file

```shell
bundle install
```

Now we'll create a `Procfile`. This is a file that both [Foreman](http://theforeman.org/) and [Heroku](https://dashboard.heroku.com/) use to manage the running process.

```yaml
factor: bundle exec factor s
```

## Deploy on Heroku

You can follow Herokus instructions for [Deploying with Git](https://devcenter.heroku.com/articles/git). This directory work as-is on Heroku.

## Deploy on your own server

Before you get the application running on a server you need to make sure it has a few prereqs.

- Ruby 2.1.3 installed, per Step 1.
- You will need to deploy your working directory to that server (e.g. `~/workflows`)
- You may need `sudo` access to the server

To start the server manually you can run this from the working directory, which starts the foreman process monitor. It will keep the workflows running and restart them in case of a crash.

```shell
foreman start
```

But to run it in the background we recommend using a process manager like Upstart on Ubuntu. Foreman provides a mechanism to set this up easily.


```shell
[sudo] foreman export upstart /etc/init -a factor
```

This will create three files.

```shell
/etc/init/factor.conf
/etc/init/factor-factor.conf
/etc/init/factor-factor-1.conf
```

Now you can run commands like `start factor`, `stop factor`, and `status factor` to manage Factor running in the background.

This will also start Factor.io whenever the server restarts or if the process crashes.
