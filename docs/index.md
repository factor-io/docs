# Getting Started
This guide gets you started with setting up your server with pre-reqs, install Factor.io server, creating your first workflow, and starting the server in the background.

## Prereqs

Factor.io requires Ruby 1.9.3. If you don't have it already installed, here is how you can get it setup on your local system. This has been tested on Ubuntu and Mac OSX.

### Mac OS
The first line installs RVM (Ruby Version Manager), which makes it easy to install ruby 1.9.3. The second line installs it and the third sets up the system to use it as the default.

    curl -sSL https://get.rvm.io | sudo bash
    source /home/ubuntu/.rvm/scripts/rvm
    rvm install ruby-1.9.3
    rvm use rvm-1.9.3 --default

### Ubuntu
Here are the commands you need to run to install/upgrade to ruby 1.9.3 on Ubuntu. This comes from "[Installing Ruby 1.9.3 on Ubuntu 12.04 Precise Pengolin (without RVM)](http://leonard.io/blog/2012/05/installing-ruby-1-9-3-on-ubuntu-12-04-precise-pengolin/)"

    sudo apt-get update
    sudo apt-get install ruby1.9.1 ruby1.9.1-dev rubygems1.9.1 irb1.9.1 ri1.9.1 rdoc1.9.1 build-essential libopenssl-ruby1.9.1 libssl-dev zlib1g-dev
    sudo update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.1 400 --slave /usr/share/man/man1/ruby.1.gz ruby.1.gz /usr/share/man/man1/ruby1.9.1.1.gz --slave /usr/bin/ri ri /usr/bin/ri1.9.1 --slave /usr/bin/irb irb /usr/bin/irb1.9.1 --slave /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.1
    sudo update-alternatives --config ruby
    sudo update-alternatives --config gem



## Download Factor.io CLI tool
The Factor.io CLI tool (`factor`), is used to run the server and manage your local environment.

    gem install factor --source https://XXXXX@gem.fury.io/skierkowski/
    factor login

## First sample workflow
Now we are going to create a basic "Hello World" workflow. This workflow will deploy a basic hello world application from Github to Heroku when you push new code to the master branch in Github.

### Create sample application
First we need to setup the basic application we will deploy to Heroku. If you do not have one, you can fork [skierkowski/hello](https://github.com/skierkowski/hello/fork) which is a very basic Sinatra-based app. We use this for our own demos too.

### Create sample workflow
Copy-and-paste this code into a new file called `github-heroku.rb` in a directory you have access to (e.g. `~/workflows/`).

    name "Github to Heroku deployment"
    id "github-heroku"
    description "Automatically deploys code from Githubs default (master) branch to Heroku"

    listen "github", "push", repo:"hello", username:'skierkowski' do |github_push_event|
      call "heroku", "deploy", app:"hello-frank", resource_id:github_push_event['resource']['id']
    end

### Under the hood
If you are familiar with Ruby, you may recognize this as Ruby Domain Specific Language (DSL). It is Ruby code, but with domain-specific implementation to describe the workflow. We'll walk through this code step-by-step.

The **name** and **id** are just to identify the workflow to humans and computers respectively.

The **description** can be markdown for rendering a quick description in the console.

The **listen** command starts the magic. This informs Factor.io to connect to a particular service and *listen* for a particulare event. In this case, we are connecting to 'github' and listening for a 'push' event. When this workflow starts Factor.io will connect to Github.com and register a post-receive web-hook. The following key/value pairs are parameters we can pass to the github::push listener. In this case we specify the username and repo. Optionally we can also specify the branch (default is master). 

The **run** command is a single call to perform an action. When the action is complete, it can execute a sub-block (not demonstrated in this code). This enables executing operations in parallel (by running them at the peer depth), or in sequence by placing the next step into the sub-block.

You can list all the available services by running `factor connectors` and you can dive into each individual service by running `factor connector describe <name>`.


## Setup service credentials
Notice that the workflow definitions don't contain any credentials. This is by design. The credentials are stored locally in your home directory (`~/.factor/credentials.yml`) as a YAML file. You can edit that file manually, or you can use the factor CLI to setup your credentials.

### Get Github Token
Go to the [Github Settings > Applications page](https://github.com/settings/applications) and in the "Personal access token" section click "Generate new token". Call it "Factor.io" and give it access to **repo**, **repo_deployment**, **user**, and **admin:repo_hook**. Once generated you'll be able to copy the new key. Once copied run this command replacing the new key.

### Get Heroku Token
Go to your [account page on Heroku](https://dashboard.heroku.com/account#api-key) and find the "API Key" section, and click "Show API Key" to get your key. Once copied, run this command.

### Set credentials

    factor credential set github
    factor credential set heroku


## Start the server!
Lets run this bad boy.


    factor run ./github-heroku.rb
    [    INFO    ] [06/Mar/2014 12:13:06] loading credentials from /Users/skierkowski/.factor/credentials.yml
    [    INFO    ] [06/Mar/2014 12:13:06] Loading workflow from /Users/skierkowski/Documents/git/factor/sample-workflows/github-heroku.rb
    [    INFO    ] [06/Mar/2014 12:13:06] Setting up workflow processor
    [    INFO    ] [06/Mar/2014 12:13:06] Starting /Users/skierkowski/Documents/git/factor/sample-workflows/github-heroku.rb
    [    INFO    ] [06/Mar/2014 12:13:06] Ctrl-c to exit
    [    INFO    ] [06/Mar/2014 12:13:07] Defining Github
    [    INFO    ] [06/Mar/2014 12:13:08]   Defining listener push
    [    INFO    ] [06/Mar/2014 12:13:09]   Defining action download_repo
    [    INFO    ] [06/Mar/2014 12:13:10] Defining Heroku
    [    INFO    ] [06/Mar/2014 12:13:11]   Defining action deploy
    [  SUCCESS   ] [06/Mar/2014 12:13:14] Workflow 'github::push' starting
    [    INFO    ] [06/Mar/2014 12:13:14]   connecting to Github
    [    INFO    ] [06/Mar/2014 12:13:14]   Checking for existing hook
    [    INFO    ] [06/Mar/2014 12:13:14]   Creating hook in Github
    [    INFO    ] [06/Mar/2014 12:13:14]   Created hook with id '1905112'
    [  SUCCESS   ] [06/Mar/2014 12:13:15] Workflow 'github::push' started
    [  WARNING   ] [06/Mar/2014 12:13:15]   Received ping for hook 'post_receive'. Not triggering a workflow since this is not a push.


Thist started the server and it is now active. It first loaded the credentials and the workflow definition. Secondly it called into the connector service to get some meta-data about the integrations which it used to generate the methods "Github.push" and "Heroku.deploy" which we use in our workflow.

Once everything was loaded it started to execute the workflow which starts with the line `Workflow 'github::push' starting` and finishes with `Workflow 'github::push' started`. This is where it called into Github and registered the web hook. You can go to your Settings > Webhooks & Services page for your repo and see there is a new URL in there starging with https://listener.factor.io/. Whenever Github gets a new webhook it automatically makes a call to that webhook with sample data, this is why there is a "WARNING" line. Since it wasn't a real event we don't execute the workflow.

## Run a workflow
Now that we have registered our workflow with Github, it is time to perform a deployment. To do this, you need to modify a file in your repo, commit it, and push the changes. You can do this in the browser on github.com. As soon as you perform the push Github will trigger the webhook, usually in <30 seconds. As soon as it does, watch the console where you are running Factor.io. You will see output like this.

    [    INFO    ] [06/Mar/2014 12:23:57]   Getting the Archive URL of the repo
    [    INFO    ] [06/Mar/2014 12:23:57]   downloading the repo from Github
    [    INFO    ] [06/Mar/2014 12:23:57]   Creating an internal resource for the repo to use in the next steps
    [    INFO    ] [06/Mar/2014 12:23:58]   Triggering workflow...
    [  SUCCESS   ] [06/Mar/2014 12:23:58] Workflow 'github::push' triggered
    [  SUCCESS   ] [06/Mar/2014 12:23:59] Action 'heroku::deploy' called
    [    INFO    ] [06/Mar/2014 12:24:00]   Getting the resource files from previous step
    [    INFO    ] [06/Mar/2014 12:24:00]   Unzipping file
    [    INFO    ] [06/Mar/2014 12:24:00]   Building the buildpack using Heroku's Anvil service
    [    INFO    ] [06/Mar/2014 12:24:41]   Pushing buildpack to Heroku
    [    INFO    ] [06/Mar/2014 12:24:54]   Release `{"release"=>"v302"}` complete
    [  SUCCESS   ] [06/Mar/2014 12:24:55] Action 'heroku::deploy' responded

Thats it! You don't need to do anything else. Now any time you push code it will be executed just like this.

In fact, you can actually modify your workflow on the spot without restarting `factor run`. It will automatically detect changes to the file and restart the workflow. You can even specify an entire directory (not just a single file) with workflows.

