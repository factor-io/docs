# Self hosting Factor.io
While it is easy to run a Factor Server, you will want to use a process manager to run the server in production. The process manager ensures that Factor.io will continue to run even in case of an unexepcted crash or system reboot.

## Running on Ubuntu with Upstart


### /etc/init/factor

    pre-start script
      mkdir -p /var/log/factor
    end

    start on runlevel [2345]
    stop on runlevel [016]
    respan

    exec bundle exec foreman start --credentials=~/factor/credentials.yml --connectors=~/factor/connectors.yml --path=~/factor >> /var/log/factor.log


    sudo start factor


## Running from command line with Foreman/Procfile
The [Foreman gem](https://github.com/ddollar/foreman) is a generic process manager which uses a Procfile to run and manage multiple process. To start foreman just run...

    cd example-workflows
    foreman start

### Running on Heroku
Heroku uses Foreman and Procfiles under the hood, so other than having the Procfile in your directory there isn't much you need to do.

    cd example-workflows
    heroku plugins:install https://github.com/ddollar/heroku-push
    heroku push --app factor-workflows
