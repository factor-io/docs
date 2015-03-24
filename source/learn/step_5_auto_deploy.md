---
title: "Step 5: Auto deployment"
category: Learn Factor.io
order: 5
---
Now that we've been basic workflows and added Heroku and Github connectors, let's put all this to work in automating deployments.

Suppose you have a Github repo, `skierkowski/hello`, and you use that to deploy an app hosted on Heroku at http://hello-frank.herokuapp.com/. Typically pushing code to Github and to Heroku are two different steps. With this workflow we want to automate the deployment. We'll create a workflow that listens for new code being pushed to the master branch in the `skierkowski/hello` repo in Github and automatically deploy it to Heroku as the `hello-frank` app.


# Create auto-deploy.rb
Now you can delete the `first-workflow.rb` file and create `auto-deploy.rb` instead.

```bash
rm first-workflow.rb
touch auto-deploy.rb
```

**auto-deploy.rb**:

```ruby
listen 'github::repos::push', repo:'skierkowski/hello' do |repo|
  run 'heroku::deploy', app:'hello-frank', content:repo.content do |deploy|
    success "Deploy complete"
  end
end
```

# Create your app in Heroku for deployment
For this tutorial we are going to deploy a basic Sinatra-based web application. You can clone [this sample app (https://github.com/skierkowski/hello)](https://github.com/skierkowski/hello) to your own account in Github.

Wherever you see `skierkowski/hello` replace it with your own Github repo.

You will also want to first deploy this application so it is created in Heroku. In this sample we use `hello-frank`, but you'll also want to use your own value.

# Start the workflow
From the working directory start `factor s`.

```bash
factor s
```

You'll see output like this...

```bash
[    INFO    ] [11/24/14 18:17:11.625] Loading workflow from /root/workflows/auto-deploy.rb
[    INFO    ] [11/24/14 18:17:11.625] Setting up workflow processor
[    INFO    ] [11/24/14 18:17:11.628] Ctrl-c to exit
[    INFO    ] [11/24/14 18:17:11.629] Starting workflow
[    INFO    ] [11/24/14 18:17:11.756] Listener 'github::push' starting
[    INFO    ] [11/24/14 18:17:11.802]   connecting to Github
[    INFO    ] [11/24/14 18:17:11.803]   Checking for existing hook
[    INFO    ] [11/24/14 18:17:11.864]   Creating hook to 'https://open-connectors.factor.io/v0.4/github/listeners/push/instances/eb.../hooks/post_receive' on skierkowski/hello.
[    INFO    ] [11/24/14 18:17:11.927]   Created hook with id '3530019'
[  SUCCESS   ] [11/24/14 18:17:11.927] Listener 'github::push' started
[    WARN    ] [11/24/14 18:17:12.641]   Incorrect branch on hook
[    WARN    ] [11/24/14 18:17:12.641]   expected: 'master', got: ''
```
You'll notice a few things here:

1. When you go to the settings for your app (in this case skierkowski/hello) in Github, you will see that Factor.io registered a new web hook. This is how Factor.io finds out from Github that a push occured to that repo.
2. When you first start the workflow you'll see that last two lines are a warning. This is because Github immidiately performs a POST with fake data to the new URL. Factor.io gets the hook call, but because it doesn't contain the appropriate information, it shows the warning and doesn't start the workflow. This is the desired behavior.
3. While the workflow has started, it has not yet peformed a deploy. It is just waiting for a Github Push event.


# Run a deployment
Now that the workflow is started (listening), and we have the web hook registered, let's perform a deployment.

In Github change the contents of your file.

