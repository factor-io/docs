---
title: Hipchat
category: Connectors
---
# Authentication
Hipchat requires a personal API Key. This key can be found under Account Settings > API Access when you login to your account. You need to get the API Key from the page and place it in your credentials.yml file.

**Note**: Hipchat also has Group API Keys under the Group Admin section of admin account, this is not the right key.

**credentials.yml**

```yaml
hipchat:
  api_key: b1488574a768a8a
```


# hipchat::message
The Room Message Listener (`hipchat::message`) listens for when someone posts a new message to a room. The filter parameter filters the messages only for ones that match the regular expression.

## Params

ID | Default | Description
--- | --- | ---
room | | The name of the room where you want to listen for messages
filter | | A regular expression filter of the messages you want to get. This also supports matching.


## Example
```ruby
listen 'hipchat::message', room:'Factor', filter:'ping (.*)' do |message|
  run 'hipchat::send', room:'Factor', message: "pong #{message.matches[0]}"
end
```


## Response example
```ruby
{
  "event" => "room_message",
  "oauth_client_id" => "69563cb5-057d-4e9d-a9db-d1bc458170a1",
  "webhook_id"      => 66824,
  "service_id"      => "hipchat",
  "listener_id"     => "room_message",
  "instance_id"     => "a842597ce18da4b17b52f4bc63d9cf89",
  "hook_id"         => 66824,
  "message"         => "go foo",
  "matches"         => [
    "foo"
  ],
  "item"  => {
    "message" => {
      "id"       => "25b9dfe9-dfc9-487e-b996-2297caaca915",
      "mentions" => [],
      "message"  => "go foo",
      "date"     => "2014-07-30T18:30:25.236745+00:00",
      "from"     => {
        "id"           => 233775,
        "mention_name" => "Skierkowski",
        "name"         => "Maciej Skierkowski",
        "links"        => {
          "self" => "https://api.hipchat.com/v2/user/233775"
        }
      }
    },
    "room" => {
      "id" => 142882,
      "name" => "Factor",
      "links" => {
        "self"     => "https://api.hipchat.com/v2/room/142882",
        "webhooks" => "https://api.hipchat.com/v2/room/142882/webhook"
      }
    }
  }
}
```

# hipchat::notification
The Room Notification Listener (`hipchat::notification`) listens for when someone sends a notification to the room. Details of returned data can be found [here](https://www.hipchat.com/docs/apiv2/webhooks#room_notification).

## Params
- **room** (required): the name of the room wher eyou want to listen for messages

## Example
```ruby
listen 'hipchat::notification', room:'Factor' do |room|
  run 'hipchat::send', room:'Factor', message: "Notification received #{room.item.message.message}"
end
```

# hipchat::exit
The Room Message Listener (`hipchat::exit`) listens for when someone posts a new message to a room.  Details of returned data can be found [here](https://www.hipchat.com/docs/apiv2/webhooks#room_exit).

## Params
- **room** (required): the name of the room wher eyou want to listen for messages

## Example

```ruby
listen 'hipchat::exit', room:'Factor' do |room|
  info "#{room.item.sender.name} exited the room"
end
```

# hipchat::enter
The Room Message Listener (`hipchat::enter`) listens for when someone posts a new message to a room. Details of returned data can be found [here](https://www.hipchat.com/docs/apiv2/webhooks#room_enter).

## Params
- **room** (required): the name of the room wher eyou want to listen for messages

## Example
```ruby
listen 'hipchat::enter', room:'Factor' do |message|
  info "#{room.item.sender.name} entered the room"
end
```

# hipchat::topic_change
The Room Message Listener (`hipchat::topic_change`) listens for when someone posts a new message to a room. Details of returned data can be found [here](https://www.hipchat.com/docs/apiv2/webhooks#room_topic_change).

## Params
- **room** (required): the name of the room wher eyou want to listen for messages

## Example

```ruby
listen 'hipchat::topic_change', room:'Factor' do |room|
  info "Room topic changed to #{room.item.topic}"
end
```

# hipchat::end
The Room Message Listener (`hipchat::send`) listens for when someone posts a new message to a room.

## Params
- **room** (required): the name of the room wher eyou want to listen for messages
- **message**: The message to post to the room.
- **format** (optional, default:'text'): The format of the message you are sending. By default this is 'text', but can also be 'html'.
- **color** (optional, default: gray): The color of the message to post. 'grey' is default, but yellow, green, red, purple are all supported.

## Example

```ruby
listen 'hipchat::message', room:'Factor', filter:'ping (.*)' do |message|
  run 'hipchat::send', room:'Factor', message:"pong #{message.matches[0]}", color: 'green'
end
```