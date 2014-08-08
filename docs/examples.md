
## Github to Heorku Deployment
This workflow registers a web-hook with Github for listening for all 'push' events. Once a push occurs it starts to deploy the application to Heroku. Once you start this workflow it will just sit there and listen, you need to push new code for the trigger to start. You will need to setup your credentials in the credentials.yml file for both Github and Heroku.

    listen 'github::push', repo:'skierkowski/hello' do |github_info|
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