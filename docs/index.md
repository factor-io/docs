# Getting Started
This guide gets you started with setting up your server with pre-reqs, install Factor.io server, creating your first workflow, and starting the server in the background.

## Prereqs

- Ruby 1.9.3+ ([Mac OSX](https://rvm.io/rvm/install), [Ubuntu](http://leonard.io/blog/2012/05/installing-ruby-1-9-3-on-ubuntu-12-04-precise-pengolin/))
- Bundler (`[sudo] gem install bundler`)

## Download Factor.io CLI tool
The Factor.io CLI tool (`factor`), is used to run the server and manage your local environment.

    gem install factor
    git clone git@github.com:factor-io/example-workflows
    cd example-workflows
    factor s

That's it. This will start the very first workflow.

## Anatomy of a workflow directory
The Factor Server, when started with `factor s` looks for at least three different files before starting the runtime. By default, the server looks for these files in the current directory `.`, however, you can specify these file locations from the commandline tool (see `factor server --help` for details). It looks for a credentials file, connectors file, and of course your workflow definition(s).


### connectors.yml
The connectors file is a YAML formatted hash of different connector identifiers and their corresponding web address. The Server and the Connectors speak a common protocol, which is exposed by these endpoints. In this file you see they are hosted on factor.io; however, you can self host your own connectors and therefore refernece your own addresses (including internal to your private network). You can [build your own connectors](/build_custom_connectors), or use one of the [Factor.io hosted workflows](/connectors).

    ---
    github: https://connectors.factor.io/v0.4/github
    heroku: https://connectors.factor.io/v0.4/heroku
    timer: https://connectors.factor.io/v0.4/timer
    web: https://connectors.factor.io/v0.4/web


### credentials.yml
The credentials file is also a YAML formatted hash of different connector IDs and a corresponding key/value pairs for each of the credentials required to the connector. For example, below we see `github` and `heroku` which are the same IDs we see in the connectors.yml file. This causes the runtime to provide these same credentials to the connector.

    ---
    github:
      api_key: 341e275d21027c7598b8182c623a7bec94d1b657
    heroku:
      api_key: c5d94xxxxxxxxx9cf0a4ce

### workflows: \*.rb
Last but not least are your workflows. A workflow is defined using special syntax, which appens to be a Ruby based domain-specific-language. You can have as many of these workflows in the directory as you would like. Each will run in parallel. [Learn more about the workflow syntax](/workflows).

    listen 'timer','every', minutes:1 do |post_info|
      run 'web','post', address:'http://requestb.in/qbx6puqb'
      run 'web','post', address:'http://requestb.in/1iu7m1d1'
      info "test"
    end
