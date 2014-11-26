---
title: Workflows
category: Factor.io Pro
---
Creating a workflow in Factor.io Pro is incredibly easy.

If you have built a workflow using the [Learn Factor.io Guide](/learn/step_1_before_you_start.html), you can just copy and paste this workflow into the `Definition` section.



# Create a sample workflow
Let's create a basic workflow to test out the functionality. Start by creating a workflow with these fields.

- **Name**: 5 minute 'hi' cron
- **Description**: Run every 5 minutes to display 'hi' in the logs.
- **Definition**:

```ruby
listen 'timer::every', minutes:5 do |timer|
  info "hi"
end
```

**Note**: You need to make sure that you have the credentials setup for each of the connectors you use in your workflow.

Now **Save** and **Start** the workflow. After a minute go have a look at the logs. You'll see something like this.

Status | Time | Message
------ | ---- | -------
INFO | 11/26/14 20:25:52.728 | Getting workflow (ca8ed3731bde3eeb) from Factor.io Cloud
INFO | 11/26/14 20:25:53.293 | Getting credentials from Factor.io Cloud
INFO | 11/26/14 20:25:53.831 | Getting connectors from Factor.io Cloud
INFO | 11/26/14 20:25:54.231 | Setting up workflow processor
INFO | 11/26/14 20:25:54.234 | Ctrl-c to exit
INFO | 11/26/14 20:25:54.235 | Starting workflow
INFO | 11/26/14 20:25:55.062 | Listener 'timer::every' starting
INFO | 11/26/14 20:25:55.572 | Starting timer every 5m
SUCCESS | 11/26/14 20:25:55.572 | Listener 'timer::every' started


# Under the hood
Under the hood the runtime that powers Factor.io Pro is the exact same code as the Factor.io Core runtime. Unlike Factor.io Core, the runtime is executed inside of a Docker container and the lifetime is managed for you.

1. **When you create the workflow** the back-end creates a new Docker-container for your instance, but it does not start it.
2. **When you start the workflow** the Docker container starts the Factor.io Core runtime using the command `factor cloud`
3. The `factor cloud` runtime retrieves Workflow, Credentials, and Connectors from the console where they were defined.
4. At this point the workflow starts just as it would if you used `factor s` on Factor.io Core.
5. **When you visit the workflow logs** the back-end pulls up the logs live from the Docker container. This may take a few seconds to load.