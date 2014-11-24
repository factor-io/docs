The Timer connector can be used to schedule periodic events either through a crontab or by a period.

## timer::cron
The Cron Listener (`timer::cron`) is used to execute a task on a schedule as defined by the crontab format. 

### Paramters
ID | Default | Description
-- | ------- | -----------
crontab | false | a 6-position definition of the schedule. The standard crontab format has 5 positions from left to right as minute, hour, day, month, and day of week. In addition to the standard format, this crontab allows you to also specify a second indicator which is in the far left position. In the example you will see that there are 6 time positions in the crontab, the first being the seconds.


### Output Prameters
- **time_run**: the time the task was triggered.

### Example
```ruby
listen 'timer::cron', crontab:'*/30 * * * * *' do |timer_info|
  info "This workflow rus every 30 seconds, this time it ran at #{timer_info['time_run']}"
end
```

## timer::every
The Every Listener (`timer::every`) executes a task every x minutes (or x hours or x seconds).

### Parameters
You are required to pass in exactly one of these values, [no more no less](https://www.youtube.com/watch?v=xOrgLj9lOwk).

ID | Default | Description
-- | ------- | -----------
hour | false | Every number of hours to run
minutes | false | Every number of minutes to run
seconds | false | Every number of seconds to run


### Output Prameters
- **time_run**: the time the task was triggered.

### Example
```ruby
listen 'timer::every', minutes:3 do |timer_info|
  info "This workflow ran at #{timer_info.time_run}"
end
```

