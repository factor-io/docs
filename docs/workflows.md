# Factor.io Workflow Syntax
The Factor.io workflow is Ruby code, but don't worry if you aren't a Rubyist. The Factor.io Workflows are defined using a Ruby Domain Specific Language. While you can use Ruby code, it only requires very basic syntax to create a workflow.


## Listen for events
Let's have a look at this example:

    listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
      puts "\#{repo_info.commiter.email']} just pushed code to console/aq-ready"
    end

First we start with `listen`. This creates a new block to listen for a particular event. When this event occurs, the inside block gets executed. The first two parameters specify the service and event name (e.g. github/push). This is followed by key value pairs which are input parameters for that listener. Some parameters are required some are optional, they are all documented for each of the services. Lastly we see `do |repo_info|`. This sends the output of the event to the parameter `repo_info`. We can use repo_info in the inside block. This parameter is a hash containing data about the particular event, this hash is also documented for each particular service.

## Run action
Let's have a look at this example:

    listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
      run 'heroku::deploy', content:repo.content, app:'hello-world' do |deploy|
        run 'hipchat::send', room:'Factor', message:"Just deployed #{deploy.version}"
        run 'gmail::send', subject:'Factor Deploy', message:"Just deployed #{deploy.version}", to:'eng@factor.io'
      end
    end


As you can see, we take action by using `run`. Like Listeners we have a similar syntax for selecting the service and action and the input parameters. Actions are incredibly similar to listeners, but with a few key differences.

Listeners execute the inside block any time the event occurs; Actions on the other hand only execute the action once and continue.

Creating an inside block is optional. In this example we run `run 'heroku','deploy'... do...` which has an inside block. Then inside of that we run `run 'hipchat','send'...` and `run 'gmail','send'...`. The second two commands won't execute until the Heroku deploy is complete. The Hipchat Send and Gmail Send will occur in parallel (async).

## Error handling and logic
Lets look here:

    listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
      run 'rspec::test', content:repo_info.content do |spec|
        if spec.stats == 'passed'
          call 'hipchat::send', room:'Factor', message:'console/qa-ready tests passed, ready to deploy'
        else
          call 'gmail::send', to:'eng@factor.io', subject:"TEST FAILED", message:spec['details']
        end
      end
    end

Since this is Ruby code we can use operations like `if`,`else`, etc. In this example we use this to check the results of a test run. If the tests pass we send a message to Hipchat to let us know it is safe to deploy. If the tests fail we send an email to the engineering team with the full test dump.

## Logging
When you run a workflow you can see the logs for the workflow. As a part of your workflow you can also post your own messages to the log. You can use `info` (white), `warn` (orange), `success` (green), and `error` (red) in your code followed by the message to send the message to the logs. 


    listen 'github::push', repo:'factor-io/console#qa-branch' do |repo_info|
      info "A Github Push just occured. This will be white"
      warn "Something went wrong, but not horribly wrong. This will be orange."
      error "Something got screwed up here. This will be red."
      success "Great success. This is green."
    end

