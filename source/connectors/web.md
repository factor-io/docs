---
title: Web
category: Connectors
connector_type: Fundamental
logo: https://raw.githubusercontent.com/factor-io/connector-web/master/logo.png
---
This connector enables you to create webhooks and call web services using POST, GET, PUT, DELETE HTTP Actions.

# web::hook
The Hook Listener (`web::hook`) generates a one-time web hook as a trigger to your workflow. This enables you to trigger your workflows by CURLing the webhook or from other 3rd party services which are capable of POSTing.

## Parameters:

ID | Default | Description
--- | ------- | -----------
id | (random) | An optional parameter to generate a static address. By default the hook address will be generated at random every time this is restarted. By specifying an ID the generate address will be `/v0.4/hooks/<hook_id>` as opposed to `/v0.4/web/listeners/hook/<random_instance_id>/hooks/<random_hook_id>`.

## Output:
When the webhook is hit with a POST it will grab the query parameters as well as the body and pass it into the workflow. The connector will attempt to deserialize the body as JSON and pass the parsed output. 

## Example
```ruby
listen 'web::hook' do |hook_info|
  info "#{hook_info.name} posted #{hook_info.message}"
end

# Now you can curl the address from the command line.
# When you start the workflow the webhook address will be generated and displayed in the output.
# curl -H "Content-Type: application/json" -d '{"name":"Ski", "message":"hello"}' https://...
```


# web::post
The Post Action (`web::post`) will make an HTTP POST to the provided address with the parameters and headers provided

ID | Default | Description
--- | ------- | -----------
params | {} | a hash of the variables to POST. This will be JSON serialized and passed as the body in the HTTP POST.
headers | {} | a hash of the HTTP Headers to pass.
url | false | the HTTP Address to POST to.


## Example
```ruby
listen 'hipchat::send_message', room:'Factor', filter:'ping (.*)' do |message|
  headers = {
    X-Auth-Token: 'foo'
  }
  vars = {
    message: message.mesage,
    from: message.item.message.from.name
  }

  run 'web::post', url:'http://requestb.in/1m1l9ak1', headers:headers, params:vars
end
```

# web::get
The Get Action (`web::get`) will make an HTTP POST to the provided address with the parameters and headers provided

## Parameters

ID | Default | Description
--- | ------- | -----------
params | {} | a hash of the variables to add as query string parameters.
headers | {} | a hash of the HTTP Headers to pass.
url | false | the HTTP Address to POST to.


## Example
```ruby
listen 'hipchat::send_message', room:'Factor', filter:'ping (.*)' do |message|
  headers = {
    X-Auth-Token: 'foo'
  }
  vars = {
    message: message.mesage,
    from: message.item.message.from.name
  }

  run 'web::get', url:'http://requestb.in/1m1l9ak1', headers:headers, params:vars
end
```