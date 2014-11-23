This connector enables you to create webhooks and call web services using POST, GET, PUT, DELETE HTTP Actions.

## Post listener (webhook)
The Post Listener (`web::post`) generates a one-time web hook as a trigger to your workflow. This enables you to trigger your workflows by CURLing the webhook or from other 3rd party services which are capable of POSTing.

### Parameters:
No input parameters are required.


### Output:
When the webhook is hit with a POST it will grab the query parameters as well as the body and pass it into the workflow. The connector will attempt to deserialize the body as JSON and pass the parsed output. 

### Example
    listen 'web::post' do |hook_info|
      info "#{hook_info.name} posted #{hook_info.message}"
    end

    # Now you can curl the address from the command line.
    # When you start the workflow the webhook address will be generated and displayed in the output.
    # curl -H "Content-Type: application/json" -d '{"name":"Ski", "message":"hello"}' https://...


## Post action
The Post Action (`web::post`) will make an HTTP POST to the provided address with the parameters and headers provided

### Parameters:
- **params** (optional, default: {}): a hash of the variables to POST. This will be JSON serialized and passed as the body in the HTTP POST.
- **headers** (optional, default: {}): a hash of the HTTP Headers to pass.
- **url** (required): the HTTP Address to POST to.

### Example
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