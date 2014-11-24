# Authentication

Authentication requires an API Key from APM.

The API Key can be found under Account Settings > Data Sharing in the API access section.

# newrelic::metrics::summarize

**Note**: This only works with a Pro New Relic account.

## Input

ID | Default | Description
-- | ------- | -----------
server |  | The Server ID to get server metrics
application |  | The Application ID to get the application metrics
range | (optional) | A Hash with the values {from:'...', to:'...''} defining the date range for the summary. If not specified gets the current values.
metrics |  | 

**Note**: The server or application IDs are required, but not both. 


## Example
```ruby
range = {
  from: 'yesterday',
  to:'now'
}
run 'newrelic::metrics::summarize', metrics: {'Apdex'=>['score']}, range:range do |metrics|
  info "stats: #{metrics}"
end
```