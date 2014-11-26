---
title: "Step 4: Getting more services"
category: Learn Factor.io
order: 4
---
Now that we've built a basic workflow, lets do something more useful and integrate with some third party services.

# Add Heroku and Github Connectors
In the past we manually changed the `connectors.yml` file. This time around lets use the `factor registry` command to add new connectors to our working directory.

Run these two commands, they will prompt you for an API Key.

```shell
factor registry connectors add heroku
factor registry connectors add github
```

The `factor registry` commands use the contents of the [Factor.io Index](https://github.com/factor-io/index) to get information about available Connectors.

When you run these commands you will see that your `connectors.yml` file will be changed and a new file `credenetials.yml` has been added.

**connectors.yml**

```ruby
---
github:
  issues: "wss://open-connectors.factor.io/v0.4/github_issue"
  repos: "wss://open-connectors.factor.io/v0.4/github_repo"
heroku: "wss://open-connectors.factor.io/v0.4/heroku"
timer: "wss://open-connectors.factor.io/v0.4/timer"
web: "wss://open-connectors.factor.io/v0.4/web"
```

The Github and Heroku connectors were added now.

**credentials.yml**

```ruby
---
github:
  api_key: b6c57341e27182c62a7be35d21027c759894d1b8
heroku:
  api_key: c5cf88930faee3bd5297fd50d4ce9a43
```
This is a new file with your credentials. This allows you to store the credentials outside of your workflow. You should add the `credentials.yml` file to your `.gitignore` file if you commit this code to git.

Earlier we used the Timer and Web connectors which were both unauthenticated. However, most other services require some form of authentication. When you run `factor s` the credential to these services are pulled out of your `credentials.yml` file and passed to the connectors.

# List available services
Github and Heroku are just a couple of the supported services. There are many more. You can list the available services by running this command.

```shell
factor registry connectors
```
