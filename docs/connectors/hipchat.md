# Hipchat

## Credentials
- **api_key**

## Room Message Listener
- **room**
- **filter**

### Response
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

