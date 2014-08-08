
## Github to Heorku Deployment
This workflow registers a web-hook with Github for listening for all 'push' events. Once a push occurs it starts to deploy the application to Heroku. Once you start this workflow it will just sit there and listen, you need to push new code for the trigger to start. You will need to setup your credentials in the credentials.yml file for both Github and Heroku.

    listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
      run 'heroku::deploy', app:'hello-frank', content:github_info.content do |deploy_info|
        info 'Deployment is complete'
      end
    end


## Basic distributed cron
This workflow runs every 2 hours (120 minutes) and simply executs `pwd` and `ls` on `sandbox.factor.io`. You need to setup SSH credentials in your credentials.yml file.

    listen 'timer::every', minutes:120 do |timer_info|
      run 'ssh::execute', host: 'ubuntu@sandbox.factor.io', commands: ['pwd', 'ls'] do |exec_info|
        info "Path is: #{exec_info.commands[0].all}"
      end
    end

## Rerun deployment on Travis-ci
This workflow runs every 2 hours (120 minutes) and simply executs `pwd` and `ls` on `sandbox.factor.io`. You need to setup SSH credentials in your credentials.yml file.

    listen 'timer::every', minutes:120 do |timer_info|
      run 'travis::redeploy', repo:'factor-io/factor' do |build_info|
        info "Rebuilding #{build_info.number}"
      end
    end

## Hipchat echo
This is a simple Hipchat bot to listen for "ping *" and respond with "pong *".

    room = 'Factor'
    listen 'hipchat::send_message', room:'Factor', filter:'ping (.*)' do |message|
      run 'hipchat::send', room:'Factor', message: "pong #{message.matches[0]}"
    end

## SSH Deploy
If you are familiar with [DeployHQ](https://www.deployhq.com/) you'll know it is capable of listening for pushes in github and then deploy them to your server via SSH. Well this demo enables you to do exactly the same thing for free. Here we listen for new code in https://github.com/skierkowski/hello in the master branch and then deploy it to a sandbox server.

    listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
      run 'ssh:upload', host:'ubuntu@sandbox.factor.io', content: github_info.content, path:'/home/ubuntu/web'
    end


## Triger SSH command on github push
This is a real scenario from one of our customers. Whenever they push new code to a repo in github they want Factor.io to connect to a server and run a shell script. This shell script handles the deployment of the application (as well as rollbacks if needed). Once it is done, it sends a notification via Pushover. We've removed the identity of the customer and replaced it with dummy data.

    listen 'github::push', repo:'skierkowski/hello' do |github_info|
      commands = []
      commands << 'cd ~/workspace/'
      commands << 'git pull origin master'
      commands << 'mg restart app'
      run 'ssh::execute', commands:commands, host:'ec2-user@sandbox.factor.io' do |ssh_info|
        run 'pushover::notify', message:'Successfully deployed WH website'
      end
    end


## Hipchat Deploy Bot
This is another user example. Whenver one of the engineers writes "FIRE MISSILES" in their hipchat room this workflow will run a script on a server which handles the deployment. (We've removed the user identity info). If you want to use a real bot check out [Hubot](https://hubot.github.com/).

    listen 'hipchat::send_message', room:'Engineering', filter:'FIRE MISSILES'
      run 'ssh::execute', commands:['sudo /opt/dio/sbin/make.sh'], host:'ubuntu@live.sandbox.factor.io' do
        success "Deployed app to live.sandbox.factor.io"
      end
    end