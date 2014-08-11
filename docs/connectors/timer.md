# Timer
The Timer connector can be used to schedule periodic events either through a crontab or by a period.

## Cron Listener
The Cron Listener (`timer::cron`) is used to execute a task on a schedule as defined by the crontab format. 

### Paramters
- **crontab**: a 6-position definition of the schedule. The standard crontab format has 5 positions from left to right as minute, hour, day, month, and day of week. In addition to the standard format, this crontab allows you to also specify a second indicator which is in the far left position. In the example you will see that there are 6 time positions in the crontab, the first being the seconds.

### Output Prameters
- **time_run**: the time the task was triggered.

### Example
    listen 'timer', 'cron', crontab:'*/30 * * * * *' do |timer_info|
      info "This workflow rus every 30 seconds, this time it ran at #{timer_info['time_run']}"
    end

## Every Listener
The Every Listener (`timer::every`) executes a task every x minutes (or x hours or x seconds).

### Parameters
You are required to pass in exactly one of these values, [no more no less](https://www.youtube.com/watch?v=xOrgLj9lOwk).

- **hour**: every number of hours to run
- **minutes**: every number of minutes to run
- **seconds**: every number of seconds to run. We don't recommend using this. Do you really need to execute this more than once a minute?

### Output Prameters
- **time_run**: the time the task was triggered.

### Example
    listen 'timer', 'every', minutes:3 do |timer_info|
      info "This workflow ran at #{timer_info.time_run}"
    end

