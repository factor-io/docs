---
title: "Step 2: Building the framework"
category: Build a custom connector
order: 2
---

Let's create a connector for the [Mailgun service](http://www.mailgun.com/), which provides an API to send and receive emails.

Each of these connectors is packaged as a gem. As such, we need to define the `gemspec` file.

**factor-connector-mailgun.gemspec**

```ruby
# encoding: UTF-8
$LOAD_PATH.push File.expand_path('../lib', __FILE__)

Gem::Specification.new do |s|
  s.name          = 'factor-connector-mailgun'
  s.version       = '0.0.1'
  s.platform      = Gem::Platform::RUBY
  s.authors       = ['Maciej Skierkowski']
  s.email         = ['maciej@factor.io']
  s.homepage      = 'https://factor.io'
  s.summary       = 'Mailgun Factor.io Connector'
  s.files         = Dir.glob('lib/factor/connector/*.rb')

  s.require_paths = ['lib']

  s.add_runtime_dependency 'factor-connector-api', '~> 0.0.13'
  s.add_runtime_dependency 'rest-client', '~> 1.7.2'
end
```

You'll need to make sure that the `name` and `summary` are updated for each spec, you'll also want to add your own `authors`, `email`.

We are going to use the `factor-connector-api` gem to specify the connector, so we need to add this as a `add_runtime_dependency` option. To call the Mailgun service we are not going to use a Ruby-gem for it, but instead, we'll use `rest-client` to make the calls, so we'll need to add this as a runtime dependency too.

**Gemfile**

```ruby
source "https://rubygems.org"
gemspec
```

Typically all of your dependencies are defined in the `Gemfile`, but in this case we define it in the gemspec file, so instead, we use this as our `Gemfile`.

Now we can run bundle install to install these dependencies

```shell
bundle exec install
```
