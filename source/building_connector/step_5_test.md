---
title: "Step 5: Testing"
category: Build a custom connector
order: 5
---

Now that we can run Factor.io in a development environment, we need to build some tests.


First, we need to update the gemspec file as we will need both `rspec` and `rake` gems.

**factor-connector-mailgun.gemspec**

```ruby
s.add_development_dependency 'rspec', '~> 3.1.0'
s.add_development_dependency 'rake', '~> 10.3.2'
```

Now add `Rakefile` so that by default when we run `rake` it runs the tests.

**Rakefile**

```ruby
# encoding: UTF-8
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new :spec do |t|
  t.rspec_opts = '--color --order random'
  t.verbose    = false
end

task default: :spec
```

Now we need to add `spec_helper` and `mailgun_spec` to add our first tests

**spec/spec_helper**

```ruby
require 'rspec'
require 'factor-connector-api/test'

Dir.glob('./lib/factor/connector/*.rb').each { |f| require f }

RSpec.configure do |c|
  c.include Factor::Connector::Test
end
```

**spec/mailgun_spec.rb**

```ruby
require 'spec_helper'

describe 'Mailgun' do
  describe 'Messages' do
    it 'can send a message' do
      service_instance = service_instance('mailgun_messages')

      params = {
        'api_key' => ENV['MAILGUN_API_KEY'],
        'to'      => 'factor-test@mailinator.com',
        'from'    => 'Factor Test <factor-test@mailinator.com>',
        'domain'  => ENV['MAILGUN_DOMAIN'],
        'subject' => "Test on #{Time.now}",
        'message' => 'Test'
      }

      service_instance.test_action('send',params) do
        response = expect_return
        expect(response[:payload]).to be_a(Hash)
        expect(response[:payload]).to include('message')
        expect(response[:payload]['message']).to be_a(String)
        expect(response[:payload]).to include('id')
        expect(response[:payload]['id']).to be_a(String)
        expect(response[:payload]['message']).to be == 'Queued. Thank you.'
      end
    end
  end
end
```

You'll need to set the `MAILGUN_API_KEY` and `MAILGUN_DOMAIN` for these tests to work. Once they do, you can run `bundle exec rake` to run the tests.
