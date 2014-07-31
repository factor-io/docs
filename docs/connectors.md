
# BitBalloon

## Deploy action

# Github

## Push listener

## Download repository action

# Heroku

## Deploy action

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

# Pushover

## Notify Action

### Credentials
- **user**:
- **password**:

### Fields:
- **message (required)**:
- **title (optional)**: (defualt: 'Factor.io')

### Response
    {
      'status':1,
      'request': '99fc262204305588f222c3445fd82364'
    }

### Example
    run 'pushover', 'notify', message: 'Whatup' do |notify_info|
      if notify_info['status'] == 1
        success "Message delivered" 
      else
        fail "Failed to deliver message"
      end
    end

# Notify Action

# SSH

## Remote Execute Action

## SCP Upload Action

# Timer

## Cron Listener
Use this to execute a task on a schedule as defined by the crontab format. The standard crontab format has 5 positions from left to right as minute, hour, day, month, and day of week. In addition to the standard format, this crontab allows you to also specify a second indicator which is in the far left position. In the example you will see that there are 6 time positions in the crontab, the first being the seconds.

### Fields
- **crontab**: a 6-position definition of the schedule

### Example
    listen 'timer', 'cron', crontab:'*/30 * * * * *' do |timer_info|
      info "This workflow rus every 30 seconds, this time it ran at #{timer_info['time_run']}"
    end

## Every Listener
Use this to execute a task every x minutes.

### Example
    listen 'timer', 'every', minutes:3 do |timer_info|
      info "This workflow ran at #{timer_info['time_run']}"
    end

# Travis

## Credentials
- **access_token**: Your Travis access token which you can obtain from the command line via `travis login`
or...
- **github_token**: Instead of a Travis token you can also use a Github token which you can obtain from Account Settings > Applications > Personal access token

## Rebuild

- **repo**: the repo slug (e.g. rails/rails) for the build you want to rebuild
- **pro**: (optional, default:false) whether you are using Travis Pro version (private repos)
- **build_number**: (optional) the build number you want to re-build. By default will use the last build.


# Web

## Post listener (webhook)

## Post action

## Get action

## Put action

## Delete action

## Head action
