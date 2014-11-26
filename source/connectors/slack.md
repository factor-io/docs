---
title: Slack
category: Connectors
---
# Authentication
Slack requires a token for all actions, you can get that at the main [api](https://api.slack.com/) page. Other common requirements are [channel](https://api.slack.com/methods/channels.list/test), and [users](https://api.slack.com/methods/users.list/test). We've set it up so that you can use you member names and channel names but you can grab any id for those there. Set up the credentials.yml with your token.

**credentials.yml**
```yml
slack:
  token: '<yourtokenhere>'
```

### Setting up the Connectors
Lets make sure we have slack in our connectors.yml file.

### connectors.yml
```yml
---
bitballoon: wss://open-connectors.factor.io/v0.4/bitballoon
bitbucket: wss://open-connectors.factor.io/v0.4/bitbucket
github: wss://open-connectors.factor.io/v0.4/github
heroku: wss://open-connectors.factor.io/v0.4/heroku
hipchat: wss://open-connectors.factor.io/v0.4/hipchat
middleman: wss://open-connectors.factor.io/v0.4/middlemanb
rackspace:
  compute: wss://open-connectors.factor.io/v0.4/rackspace_compute
  flavors: wss://open-connectors.factor.io/v0.4/rackspace_images
  images: wss://open-connectors.factor.io/v0.4/rackspace_flavors
slack:
  channel: wss://open-connectors.factor.io/v0.4/slack_channel
  chat: wss://open-connectors.factor.io/v0.4/slack_chat
  group: wss://open-connectors.factor.io/v0.4/slack_group
  user: wss://open-connectors.factor.io/v0.4/slack_user
ssh: wss://open-connectors.factor.io/v0.4/ssh
timer: wss://open-connectors.factor.io/v0.4/timer
web: wss://open-connectors.factor.io/v0.4/web
```

# slack::chat::send
This is an action to send a message to slack. Required parameters are token, channel, and text.

## Input
- **channel** (required, default: nil): You can use channel names, alternatively you can get your [channel](https://api.slack.com/methods/channels.list/test) id there.

- **text** (required, default: nil): The content of your message.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "channel"=>"C02P77YSN", "ts"=>"1414442450.000040"}}
```

## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::chat::send', channel: '<channel>', text: '<Save the Whales>'
end
```

# slack::channel::list
This is an action to list out all your slack channels. Required parameter is token.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "channels"=>[{"id"=>"C02P77YSN", "name"=>"andyetanotherchannel", "is_channel"=>true, "created"=>1412957885, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_general"=>false, "is_member"=>true, "members"=>["U02NS7PQB", "U02NS9RRK", "U02NTMSFG"], "topic"=>{"value"=>"text", "creator"=>"U02NS7PQB", "last_set"=>"1414441391"}, "purpose"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}, "num_members"=>3}, {"id"=>"C02PW1VFR", "name"=>"braging", "is_channel"=>true, "created"=>1413403277, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_general"=>false, "is_member"=>true, "members"=>["U02NS7PQB"], "topic"=>{"value"=>"text", "creator"=>"U02NS7PQB", "last_set"=>"1413484126"}, "purpose"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}, "num_members"=>1}, {"id"=>"C02NS7PQR", "name"=>"general", "is_channel"=>true, "created"=>1412787384, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_general"=>true, "is_member"=>true, "members"=>["U02NS7PQB", "U02NS9RRK", "U02NTMSFG"], "topic"=>{"value"=>"text", "creator"=>"U02NS7PQB", "last_set"=>"1413408854"}, "purpose"=>{"value"=>"This channel is for team-wide communication and announcements. All team members are in this channel.", "creator"=>"", "last_set"=>"0"}, "num_members"=>3}, {"id"=>"C02P77YFL", "name"=>"otherchannel", "is_channel"=>true, "created"=>1412957877, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_general"=>false, "is_member"=>true, "members"=>["U02NS7PQB", "U02NTMSFG"], "topic"=>{"value"=>"text", "creator"=>"U02NS7PQB", "last_set"=>"1413403235"}, "purpose"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}, "num_members"=>2}, {"id"=>"C02NS7PQX", "name"=>"random", "is_channel"=>true, "created"=>1412787384, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_general"=>false, "is_member"=>true, "members"=>["U02NS7PQB", "U02NS9RRK", "U02NTMSFG"], "topic"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}, "purpose"=>{"value"=>"A place for non-work banter, links, articles of interest, humor or anything else which you'd like concentrated in some place other than work-related channels.", "creator"=>"", "last_set"=>"0"}, "num_members"=>3}]}}
```

## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::channel::list'
end
```

# slack::channel::invite
This is an action to send a message to slack. Required parameters are token, channel, and text.

## Input
- **channel** (required, default: nil): You can use channel names, alternatively you can get your [channel](https://api.slack.com/methods/channels.list/test) id there.

- **user** (required, default: nil): You can user names, alternatively you can get your [user](https://api.slack.com/methods/users.list/test) id there.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>false, "error"=>"cant_invite_self"}}
```
## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::channel::invite', channel: '<channel>', user: '<user>'
end
```

# slack::channel::History
This is an action to list out the message history of a slack channel. Required parameters are token and channel.

## Input
- **channel** (required, default: nil): You can use channel names, alternatively you can get your [channel](https://api.slack.com/methods/channels.list/test) id there.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "messages"=>[{"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414442084.000036"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414442050.000035"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414442044.000034"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414442030.000033"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414442028.000032"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414441396.000031"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414441391.000030"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414441209.000029"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414441207.000028"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414441174.000027"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414441134.000026"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414441008.000025"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414441006.000024"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440814.000023"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440814.000022"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440783.000021"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440777.000020"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440760.000019"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440755.000018"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440633.000017"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440623.000016"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440544.000015"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440534.000014"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440505.000013"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440500.000012"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440420.000011"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440413.000010"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440340.000009"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440334.000008"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440283.000007"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440199.000006"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440137.000005"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440082.000004"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1414440077.000003"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414440034.000002"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1414439933.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413833056.000000"}, {"user"=>"U02NS9RRK", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NS9RRK|skierkowski> has joined the channel", "ts"=>"1413833051.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413832781.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413832702.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413832617.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413558505.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413558500.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413558424.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413558418.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413557063.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413557060.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413557002.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413556994.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413556944.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413556941.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413556866.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413556860.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413554914.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413554913.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413554566.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413554561.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413554460.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413554459.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413554300.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413554297.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413553985.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413553981.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413553620.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413553617.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413553428.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413553411.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413553410.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413553209.000000"}, {"user"=>"U02NTMSFG", "type"=>"message", "subtype"=>"channel_leave", "text"=>"<@U02NTMSFG|jozw> has left the channel", "ts"=>"1413553208.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413553207.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413553203.000000"}, {"user"=>"U02NTMSFG", "type"=>"message", "subtype"=>"channel_leave", "text"=>"<@U02NTMSFG|jozw> has left the channel", "ts"=>"1413553141.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413553140.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413553138.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413553136.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413553051.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413553047.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413552889.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413552885.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413552803.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413552788.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413552716.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413552703.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413552534.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413552520.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413552474.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413552461.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413550545.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413550529.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413550527.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413550462.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413550458.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413550443.000000"}, {"text"=>"text", "username"=>"bot", "type"=>"message", "subtype"=>"bot_message", "ts"=>"1413550416.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413550403.000000"}, {"user"=>"U02NTMSFG", "type"=>"message", "subtype"=>"channel_leave", "text"=>"<@U02NTMSFG|jozw> has left the channel", "ts"=>"1413550400.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413550399.000000"}, {"user"=>"U02NTMSFG", "inviter"=>"U02NS7PQB", "type"=>"message", "subtype"=>"channel_join", "text"=>"<@U02NTMSFG|jozw> has joined the channel", "ts"=>"1413550339.000000"}, {"user"=>"U02NS7PQB", "topic"=>"text", "type"=>"message", "subtype"=>"channel_topic", "text"=>"<@U02NS7PQB|andrew> set the channel topic: text", "ts"=>"1413550335.000000"}], "has_more"=>true}}
```

## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::channel::history', channel: '<channel>'
end
```

# slack::channel::topic
This is an action to change the topic of a slack channel. Required parameters are token, channel and your new topic.

## Input
- **channel** (required, default: nil): You can use channel names, alternatively you can get your [channel](https://api.slack.com/methods/channels.list/test) id there.

- **topic** (required, default: nil): What your topic will change to.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "topic"=>"We have saved the whales!"}}
```
## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::channel::topic', channel: '<channel>', topic: '<topic>'
end
```

# slack::user::list
This is an action to list out all your users. Required parameter is token.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "members"=>[{"id"=>"U02NS7PQB", "name"=>"andrew", "deleted"=>false, "status"=>nil, "color"=>"9f69e7", "real_name"=>"", "skype"=>nil, "phone"=>nil, "tz"=>"America/Los_Angeles", "tz_label"=>"Pacific Daylight Time", "tz_offset"=>-25200, "profile"=>{"real_name"=>"", "real_name_normalized"=>"", "email"=>"email@gmail.com", "image_24"=>"https://secure.gravatar.com/avatar/a9ab3c0ad263f02e9b9042c3d6f2b724.jpg?s=24&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0008-24.png", "image_32"=>"https://secure.gravatar.com/avatar/a9ab3c0ad263f02e9b9042c3d6f2b724.jpg?s=32&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0008-32.png", "image_48"=>"https://secure.gravatar.com/avatar/a9ab3c0ad263f02e9b9042c3d6f2b724.jpg?s=48&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0008-48.png", "image_72"=>"https://secure.gravatar.com/avatar/a9ab3c0ad263f02e9b9042c3d6f2b724.jpg?s=72&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0008-72.png", "image_192"=>"https://secure.gravatar.com/avatar/a9ab3c0ad263f02e9b9042c3d6f2b724.jpg?s=192&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0008.png"}, "is_admin"=>true, "is_owner"=>true, "is_primary_owner"=>true, "is_restricted"=>false, "is_ultra_restricted"=>false, "is_bot"=>false, "has_files"=>true}, {"id"=>"U02NTMSFG", "name"=>"jozw", "deleted"=>false, "status"=>nil, "color"=>"4bbe2e", "real_name"=>"", "skype"=>nil, "phone"=>nil, "tz"=>"America/Los_Angeles", "tz_label"=>"Pacific Daylight Time", "tz_offset"=>-25200, "profile"=>{"real_name"=>"", "real_name_normalized"=>"", "email"=>"email@gmail.com", "image_24"=>"https://secure.gravatar.com/avatar/3a967a03d32182c19040f5fdc27eb964.jpg?s=24&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0024-24.png", "image_32"=>"https://secure.gravatar.com/avatar/3a967a03d32182c19040f5fdc27eb964.jpg?s=32&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0024-32.png", "image_48"=>"https://secure.gravatar.com/avatar/3a967a03d32182c19040f5fdc27eb964.jpg?s=48&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F20655%2Fimg%2Favatars%2Fava_0024-48.png", "image_72"=>"https://secure.gravatar.com/avatar/3a967a03d32182c19040f5fdc27eb964.jpg?s=72&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0024-72.png", "image_192"=>"https://secure.gravatar.com/avatar/3a967a03d32182c19040f5fdc27eb964.jpg?s=192&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0024.png"}, "is_admin"=>false, "is_owner"=>false, "is_primary_owner"=>false, "is_restricted"=>false, "is_ultra_restricted"=>false, "is_bot"=>false, "has_files"=>false}, {"id"=>"U02NS9RRK", "name"=>"skierkowski", "deleted"=>false, "status"=>nil, "color"=>"e7392d", "real_name"=>"", "skype"=>nil, "phone"=>nil, "tz"=>"America/Los_Angeles", "tz_label"=>"Pacific Daylight Time", "tz_offset"=>-25200, "profile"=>{"real_name"=>"", "real_name_normalized"=>"", "email"=>"email@factor.io", "image_24"=>"https://secure.gravatar.com/avatar/4640a9f5a66dc97a0fc30ba69b412abc.jpg?s=24&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0012-24.png", "image_32"=>"https://secure.gravatar.com/avatar/4640a9f5a66dc97a0fc30ba69b412abc.jpg?s=32&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0012-32.png", "image_48"=>"https://secure.gravatar.com/avatar/4640a9f5a66dc97a0fc30ba69b412abc.jpg?s=48&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0012-48.png", "image_72"=>"https://secure.gravatar.com/avatar/4640a9f5a66dc97a0fc30ba69b412abc.jpg?s=72&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0012-72.png", "image_192"=>"https://secure.gravatar.com/avatar/4640a9f5a66dc97a0fc30ba69b412abc.jpg?s=192&d=https%3A%2F%2Fslack.global.ssl.fastly.net%2F8390%2Fimg%2Favatars%2Fava_0012.png"}, "is_admin"=>false, "is_owner"=>false, "is_primary_owner"=>false, "is_restricted"=>false, "is_ultra_restricted"=>false, "is_bot"=>false, "has_files"=>false}]}}
```
## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::user::list'
end
```

# slack::group::create
This is an action to a new private group in slack. Required parameters are your token and the new group name.

## Input
- **name** (required, default: nil): Name of your new group.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "group"=>{"id"=>"G02RYKEAY", "name"=>"text-7274", "is_group"=>true, "created"=>1414444296, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_open"=>true, "last_read"=>"0000000000.000000", "latest"=>nil, "unread_count"=>0, "members"=>["U02NS7PQB"], "topic"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}, "purpose"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}}}}
```

## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::group::create',  name: '<name>'
end
```

# slack::group::invite
This is an action to a new private group in slack. Required parameters are your token and the new group name.

## Input
- **channel** (required, default: nil): You can use channel names, alternatively you can get your [channel](https://api.slack.com/methods/channels.list/test) id there.

- **user** (required, default: nil): You can user names, alternatively you can get your [user](https://api.slack.com/methods/users.list/test) id there.

## Output
Output is a ruby hash.
```ruby
{:type=>"return", :payload=>{"ok"=>true, "group"=>{"id"=>"G02RYKUFN", "name"=>"text-5762", "is_group"=>true, "created"=>1414444411, "creator"=>"U02NS7PQB", "is_archived"=>false, "is_open"=>true, "last_read"=>"1414444411.000002", "latest"=>{"user"=>"U02NS7PQB", "type"=>"message", "subtype"=>"group_join", "text"=>"<@U02NS7PQB|andrew> has joined the group", "ts"=>"1414444411.000002"}, "unread_count"=>0, "members"=>["U02NS7PQB", "U02NTMSFG"], "topic"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}, "purpose"=>{"value"=>"", "creator"=>"", "last_set"=>"0"}}}}
```

## Example
```ruby
listen 'timer::every', minutes:1 do |post_info|
  run 'slack::group::invite', name: '<name>'
end
```