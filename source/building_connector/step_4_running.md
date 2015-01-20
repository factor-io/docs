---
title: "Step 4: Running an action"
category: Build a custom connector
order: 4
---

Now that you have a basic implementation, we need to connect everything together with the **Connector Service** and **Factor.io Runtime**

# Build the gem
First, we need to build the gem and install it locally.

```shell
bundle install
gem build factor-connector-mailgun.gemspec
gem install factor-connector-mailgun-0.0.1.gem
```

# Get Connector Service

Next, lets get the **Connector Service** from Github.

```shell
git clone git@github.com:factor-io/connector
cd connector
```

# Install the gem in Connector Service
Now update the Gemfile to include the new connector we created and run `bundle install`.

**Gemfile**

```ruby
gem 'factor-connector-mailgun'
```

```shell
bundle install
```

**Notes**: This is going to fail because there is already a connector called `factor-connector-mailgun`, so for your own service you'll want to create something with a different name.

**init.rb**

```ruby
require_relative '../factor-connector-yourconnector/lib/factor/connector/mailgun'
```

**Note**: even though we updated our Gemfile and ran `bundle install`, we are not using the library from the Gem. Instead, we reference our source code directly. We needed to update the Gemfile and run `bundle install` such that the dependent gems would get installed. You'll need to re-run the steps on this page every time you update your dependency list.

# Run the connector service

We are now ready to run the Connector service.

```shell
bundle exec rackup
```

And now you should see

```shell
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 0.0.0.0:9292, CTRL+C to stop
```

# Setup the Runtime working directory
While keeping the Connector Service running, open another terminal window. Go to your `workflows` directory created in the [Learn Factor.io Tutorial](/learn/step_2_your_first_command.html).

Login to your Mailgun Account and get the API Key and Domain and put them into the **credentials.yml** file.

**credentials.yml**

```yaml
mailgun:
  api_key: ...
  domain: ...
```

**connectors.yml**

```yaml
mailgun:
  messages: ws://localhost:9292/v0.4/mailgun_messages
```

# Run, Factor Run!
```shell
factor run mailgun::messages::send '{"to":"maciej@factor.io","from":"maciej@factor.io","subject":"hello","mesages":"world"}'
```
