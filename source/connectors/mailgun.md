---
title: Mailgun
category: Connectors
connector_type: Email
logo: https://raw.githubusercontent.com/factor-io/connector-mailgun/master/logo.png
---
# Authentication
The API Key can be found on the [Mailgun Control Panel](https://mailgun.com/cp) under 'API Key'. You will also need the Domain which is the value for 'Domain Name' in the [Domains tab in the control panel](https://mailgun.com/app/domains).

**credentials.yml**

```yaml
---
mailgun:
  api_key: key-4567546785678
  domain: factor.io
```

# Messages

## Send a message
### mailgun::messages::send
Send an email message.

### Parameters
ID | Default | Description
--- | --- | ---
to | (required) | The 'to' email address.
subject | (required) | The email subject line.
message | (required) | The email message.

### Example
```ruby
listen 'timer::cron', crontab:'0 0 * * *' do |ping|
  message = "Today is #{DateTime.now.strftime('%Y-%m-%d')}"
  run 'mailgun::messages::send', to:'me@factor.io', subject:'Today', message:message
end
```