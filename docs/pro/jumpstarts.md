A **Jumpstart** is a template for workflows. It makes it easy for your users to create workflows from a template, instead of having to write the code themselves. To define a Jumpstart you have to define the template for the Workflow definitions. Additionally you have to define the variables you will prompt the user, which will be placed in the template when the workflow runs.

This is an **admin only** feature so you, as the owner of Factor.io Pro, can manage Jumpstarts. The members and owners (**users**) in the organizations can create workflows using these jumpstarts, but they cannot modify them.

# Without jumpstarts
Suppose you created a workflow called "Github to Heroku Deployment", and you want to turn it into a Jumpstart so all of your users can easily use this same workflow.

```ruby
listen 'git::push', repo:'skierkowski/hello' do |repo|
  run 'heroku::deploy', app:'hello-frank', content: repo.content do |deploy|
    success "Deploy complete"
  end
end
```

Given this workflow, you'll notice that the `repo` and `app` options in this workflow are specific to your instance. These are the fields that we will want to abstract away and prompt the user for those values when they create the workflow using this jumpstart.

# Defining the template
Start by creating a new Jumpstart. Provide the `Name`, "Github to Heroku Deployment", and add a `Description`.

For the `Workflow Template` field, add this block of code.

```ruby
listen 'git::push', repo:'{{repo}}' do |repo|
  run 'heroku::deploy', app:'{{app}}', content: repo.content do |deploy|
    success "Deploy complete"
  end
end
```

You'll notice that we replace `skierkowski/hello` with `{{repo}}`, and `hello-frank` with `{{app}}`.

This template uses [Liquid Markup](http://liquidmarkup.org/) to create placeholders for those values.

Next, we need to configure the jumpstart to prompt the user for the values of `repo` and `app`.


# Define the variables
In the `Variables` section, add these two values.

ID | Name | Description
-- | ---- | -----------
repo | Github Repo address | If your Github repo address is https://github.com/skierkowski/hello, then this value is skierkowski/hello.
app | Heroku App | You Heroku app name of this app you would like to deploy.

Now go ahead and save this new Jumpstart.

# Create a workflow using the Jumpstart
Go to workflows page. You will notice that "Create Workflow" is now a drop down. On this list you will see "Github to Heroku Deployment". You will see that this prompts you for "Github Repo address" and "Heroku App", just like we specified in the variables section of the jumpstart. Go ahead and save it and run it.

# What you need to know about Jumpstarts
- When you modify the jumpstart, you have to restart the workflows utilizing this jumpstart for the changes to take affect. We'll add a tool to restart all impacted workflows.
- When you edit the workflow, you also have to restart it for the new values to take effect.
- The name and description of the workflow are defined by the jumpstart. When they change in the jumpstart they are immidiately changes for the workflows.

