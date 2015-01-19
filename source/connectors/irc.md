---
title: IRC
category: Connectors
logo: https://raw.githubusercontent.com/factor-io/connector-irc/master/logo.png
---

# Authentication

# Messages

## Send a message
### irc::messages::send

### Parameters

ID | Default | Description
--- | --- | ---
server | (required) | The host name of the IRC Server
channel | (required) | The channel to send the message to.
message | (required) | Message to send to the channel
user | (required) | The username
nick | (same value as user) | The nickname you would like to identify the bot


### Output
Nothing. If it succeeded it will return.