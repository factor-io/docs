---
title: Workflow Syntax
category: Guides
status: revise
---
The Factor.io workflow is Ruby code, but don't worry if you aren't a Rubyist. The Factor.io Workflows are defined using a Ruby Domain Specific Language. While you can use Ruby code, it only requires very basic syntax to create a workflow.


## Listen for events
Let's have a look at this example:

```ruby
listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
  puts "\#{repo_info.commiter.email']} just pushed code to console/aq-ready"
end
```

First we start with `listen`. This creates a new block to listen for a particular event. When this event occurs, the inside block gets executed. The first two parameters specify the service and event name (e.g. github/push). This is followed by key value pairs which are input parameters for that listener. Some parameters are required some are optional, they are all documented for each of the services. Lastly we see `do |repo_info|`. This sends the output of the event to the parameter `repo_info`. We can use repo_info in the inside block. This parameter is a hash containing data about the particular event, this hash is also documented for each particular service.

## Run action
Let's have a look at this example:

```ruby
listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
  run 'heroku::deploy', content:repo.content, app:'hello-world' do |deploy|
    run 'hipchat::send', room:'Factor', message:"Just deployed #{deploy.version}"
    run 'gmail::send', subject:'Factor Deploy', message:"Just deployed #{deploy.version}", to:'eng@factor.io'
  end
end
```


As you can see, we take action by using `run`. Like Listeners we have a similar syntax for selecting the service and action and the input parameters. Actions are incredibly similar to listeners, but with a few key differences.

Listeners execute the inside block any time the event occurs; Actions on the other hand only execute the action once and continue.

Creating an inside block is optional. In this example we run `run 'heroku','deploy'... do...` which has an inside block. Then inside of that we run `run 'hipchat','send'...` and `run 'gmail','send'...`. The second two commands won't execute until the Heroku deploy is complete. The Hipchat Send and Gmail Send will occur in parallel (async).

## Error handling and logic
Lets look here:

```ruby
listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
  run 'rspec::test', content:repo_info.content do |spec|
    message = "#{repo_info.repo} #{spec.stats}: #{spec.details}"
    call 'hipchat::send', room:'Factor', message:message
  end.on_fail do |err|
    message = "There was an error running RSpec: #{err.message || '(no msg)'}"
    call 'hipchat::send', room:'Factor', message:message
  end
end
```

As you can see from the above block, you can use `.on_fail` at the end of a block to catch failures. When a connector runs `fail` it will lead to running the `on_fail` block. 

Connectors will use `fail` often when they catch some kind of error, like a missed parameter, failed connection to a 3rd party service, incorrect credentials, etc. In such cases, the default successful block will not execute and instead the fail block will. This also occurs if there is an unhandled error in the connector.


## Logging
When you run a workflow you can see the logs for the workflow. As a part of your workflow you can also post your own messages to the log. You can use `info` (white), `warn` (orange), `success` (green), and `error` (red) in your code followed by the message to send the message to the logs. 

```ruby
listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
  info "A Github Push just occured. This will be white"
  warn "Something went wrong, but not horribly wrong. This will be orange."
  error "Something got screwed up here. This will be red."
  success "Great success. This is green."
end
```

## Organizing using 'workflow'
Once you start creating more complex orchestrations you'll notice that the nesting becomes difficult to read. As such, we provide a way to package repeatable workflow sequences into their own `workflow` blocks. 

```ruby
workflow 'bootstrap' do |options|
  run 'ssh::upload', host:options.host, content:options.content, path:'/var/factor/app'
    run 'ssh::execute', host:options.host, commands: ['./deploy']
  end
end

listen 'github::push', repo:'skierkowski/hello' do |gh|
  run 'workflow::bootstrap', host:'root@sandbox.factor.io', content:gh.content
end
```

Using the above syntax you can wrap a sequence of work in a `workflow` block. You can call this block with `run` command by addressing it with `workflow::(workflow name)`.

