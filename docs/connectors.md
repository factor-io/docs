
# BitBalloon

# BitBucket

# Github

# Heroku

# Hipchat

# Middleman

# SSH

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

# Web

## Post listener (webhook)

## Post action

## Get action

## Put action

## Delete action

## Head action
