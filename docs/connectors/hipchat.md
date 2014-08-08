# Hipchat

## Authentication
Hipchat requires a personal API Key. This key can be found under Account Settings > API Access when you login to your account. You need to get the API Key from the page and place it in your credentials.yml file.

**Note**: Hipchat also has Group API Keys under the Group Admin section of admin account, this is not the right key.

**credentials.yml**
    hipchat:
      api_key: 


## Room Message Listener
The Room Message Listener (`hipchat::send_message`) listens for when someone posts a new message to a room. The filter parameter filters the messages only for ones that match the regular expression.

### Params
- **room** (required): the name of the room wher eyou want to listen for messages
- **filter** (required): A regular expression filter of the messages you want to get. This also supports matching.

### Example
    listen 'hipchat::send_message', room:'Factor', filter:'ping (.*)' do |message|
      run 'hipchat::send', room:'Factor', message: "pong #{message.matches[0]}"
    end


### Response example
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

## Room Notification Listener

## Room Exit Listener

## Room Enter Listener

## Room Topic Change Listener

## Send Message Action

