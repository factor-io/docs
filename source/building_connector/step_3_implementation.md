---
title: "Step 3: Implement integration"
category: Build a custom connector
order: 3
---

Now that we have the basic framework setup, lets add some code to perform the integration

**lib/factor/connector/mailgun_messages.rb**

```ruby
require 'factor-connector-api'
require 'uri'
require 'rest-client'

Factor::Connector.service 'mailgun_messages' do

  action 'send' do |params|
    domain  = params['domain']
    api_key = params['api_key']
    to      = params['to']
    from    = params['from']
    subject = params['subject']
    message = params['message']


    fail 'Domain (domain) is required' unless domain
    fail 'API Key (api_key) is required' unless api_key
    fail 'To (to) is required' unless to
    fail 'Subject (subject) is required' unless subject
    fail 'Message (message) is required' unless message

    base_url     = "https://api.mailgun.net/v2/#{domain}"
    uri          = URI(base_url)
    uri.path     = uri.path + "/messages"
    uri.user     = 'api'
    uri.password = api_key

    content = { from: from, to: to, subject: subject, text: message }

    begin
      response = JSON.parse(RestClient.post(uri.to_s, content))
    rescue => ex
      fail "Failed to connect to mailgun server: #{ex.message}"
    end

    action_callback response
  end
end
```

You'll notice a number of functions which are unique to this implementation. These are the methods that come from the [Connector API](https://github.com/factor-io/connector-api).

## Factor::Connector.service
This is the primary method which defines the service. The first parameter is the name of the service we want to use. In this case we are defining `mailgun_messages`. This must match the name of the file, in this case this must be `mailgun_messages.rb`. Secondly, for consistency, it should be located in `/lib/factor/connector/` path.

Other than the first parameter, you'll notice that everything else is placed inside of the main block.

## action
The `action` method defines the action you want to make available. In this case we call it `send`. Inside the block it takes a |params| option, which  includes all the parameters passed from the Factor.io Runtime. Inside this block we will define the code to perform the work of sending a email message using Mailgun.

## info, warn, error
There are three methods, `info`, `warn`, `error` you can use in your code to log output. These are just for information purposes, so the code will continue to execute.

## fail
The fail method is like throwing an exception. It will immediately stop execution of this connector, and the workflow using this connector. Use this for validating user input from of the params.

## action_callback
Once you complete the work you have to return the results using `action_callback`. This returns any contents you want to pass back to the workflow. In this case we want to send the message response from Mailgun. This must be an Array or Hash.
