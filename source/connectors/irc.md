---
title: IRC
category: Connectors
logo: https://raw.githubusercontent.com/factor-io/connector-irc/master/logo.png
---

# Authentication
To authenticate to the server you need to specify the server address and user. These are to be obtained from the IRC provider.

**connectors.yml**
```yaml
---
irc:
  server: irc.freenode.net
  user: factorbot
```

# Messages

## Send a message
### irc::messages::send

### Parameters

ID | Default | Description
--- | --- | ---
channel | (required) | The IRC Channel for sending the message (e.g. #factortest)
message | (required) | Message to send to the channel
nick | (same value as user) | The nickname you would like to identify the bot


### Output
Nothing. If it succeeded it will return.