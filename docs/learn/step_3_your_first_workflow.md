In previous steps we ran commands from the command line. However, in most cases you will want to execute a number of actions and listen for events. To do this, we will now create our first workflow.


## Add the Timer Connector to conenctors.yml
In the last example we used the `Web` connector, now we want to add the `Timer` connector.

```yaml
---
web: wss://open-connectors.factor.io/v0.4/web
timer: wss://open-connectors.factor.io/v0.4/timer
```

## Create first-workflow.rb
In our working directory, `workflows`, we'll add one file `first-workflow.rb`. Notice that this file is called `first-workflow.rb`, which indicates it is a Ruby file. The workflow definition format is a ruby-based "Domain Specific Language." This means that you can use both Factor.io functionality mixed with Ruby functionality.

**first-workflow.rb**
```ruby
listen 'timer::every', minutes:1 do |time_info|
  info "tick tock at #{time_info.time_run}"
  run 'web::post', url: 'http://requestb.in/1h15g3i1' do |post_info|
    info 'post complete'
  end
end
```

## Analyze the workflow
While this is a small workflow, there is a lot going on, so lets analyze each block one-by-one.

```ruby
listen 'timer::every', minutes:1 do |time_info|
  #...
end
```

This first block uses the `listen` directive. This instructs the runtime to listen for a particular event. Once the workflow starts it will keep running indefinitely until you stop the workflow. The block contained inside will be executed every time the event occurs. In this case we are listening to a timer which gets executed every 1 minute.

The first option `timer::every` tells us which connector to use and which event to listen. The `timer` is identified in the `connectors.yml` file and references the Timer Connector Service.

The second option, `minutes:1`, is a set of parameters we pass to the listener to start it. We are only passing one option, but we can pass many parameters.

Lastly, we have `do |time_info|...end`. This creates a new parameter called `time_info` which contains variables provided by the listener with information about the event.

```ruby
info "tick tock at #{time_info.time_run}"
```

Now we call `info` which is used to log some information. We can also use `warn` which is displayed in orange, `error` in red, and `success` in green.

The reference to `time_info.time_run` gets the value of `time_run` from `time_info`, which was passed in from the listener when the event was triggered.

```ruby
run 'web::post', url: 'http://requestb.in/1h15g3i1' do |post_info|
  info 'post complete'
end
```
This should look familiar. As you can see we use `run` to call a specific action, in this case `web::post` with the `url` parameter. As you can see, both `run` and `listen` use the same pattern as the first option is the referene to our connector + action/event, the following paramters are the options to pass to that action/listener, followed by a new block of code.

The `run` action will execute the work one time, once the work is complete it returns some information and calls the inside block. 

## Run the Factor.io Server
Now that we have a `connectors.yml` and `first-workflow.rb` file defined in our `workflows` directory, lets start the Factor.io runtime.

```shell
factor s
```

When you run this command, you should see output like this.

```shell
[    INFO    ] [11/20/14 14:29:51.654] Loading workflow from /Users/skierkowski/Documents/git/test/first-workflow.rb
[    INFO    ] [11/20/14 14:29:51.654] Setting up workflow processor
[    INFO    ] [11/20/14 14:29:51.656] Ctrl-c to exit
[    INFO    ] [11/20/14 14:29:51.656] Starting workflow
[    INFO    ] [11/20/14 14:29:52.108] Listener 'timer::every' starting
[    INFO    ] [11/20/14 14:29:52.218]   Starting timer every 1m
[  SUCCESS   ] [11/20/14 14:29:52.218] Listener 'timer::every' started
[    INFO    ] [11/20/14 14:30:52.453]   Trigger time at 2014-11-20 22:30:52 +0000
[  SUCCESS   ] [11/20/14 14:30:52.454] Listener 'timer::every' triggered
[    INFO    ] [11/20/14 14:30:52.454] tick tock at 2014-11-20 22:30:52 +0000
[    INFO    ] [11/20/14 14:30:52.947] Action 'web::post' starting
[    INFO    ] [11/20/14 14:30:53.064]   Posting to `http://requestb.in/1h15g3i1`
[  SUCCESS   ] [11/20/14 14:30:53.113] Action 'web::post' responded
[    INFO    ] [11/20/14 14:30:53.113] post complete
```

Based on the workflow definition, the last 7 lines will be logged over and over again every minute.

### [Next: Getting more services](learn/step_4_getting_more_services)
#### [Previous: Before you start](learn/step_2_your_first_command)