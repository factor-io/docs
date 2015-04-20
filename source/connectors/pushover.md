---
title: Pushover
category: Connectors
connector_type: Notifications
logo: https://raw.githubusercontent.com/factor-io/connector-pushover/master/logo.png
---
Pushover is a simple notification service which you can use to send notifications to your mobile device.

# Authentication
You will need the Username (username) and API Key (api\_key) from Pushover. The username can be found under the "Your User Key" section after you login to [Pushover](https://pushover.net). To get the API Key first you need to create an Application by followin the steps on "Register an Application". After you register, you will get an "API Key" for that applicaiton. The username and api_key are to be placed in the credentials.yml file.

**credentials.yml**

```yaml
pushover:
  username: NFOhPbj45iTHBXlZhqpw
  api_key: C9SwnAK55Px2BlgnTo
```

# pushover::notify
The Notify Action (`pushover::notify`) sends messages to your Pushover clients.

## Parameters
ID | Default | Description
--- | --- | ---
message | | The message to send to your device
title | 'Factor.io' | Optional title of the message

## Response
```ruby
{
  'status'  => 1,
  'request' => '99fc262204305588f222c3445fd82364'
}
```

## Example
```ruby
listen 'github::push', repo:'skierkowski/hello' do |repo_info|
  run 'pushover::notify', message: "#{repo_info.repository.full_name} has new code" do |notify_info|
    if notify_info.status == 1
      success "Message delivered" 
    else
      fail "Failed to deliver message"
    end
  end
end
```
