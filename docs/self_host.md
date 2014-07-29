# Self hosting Factor.io
While it is easy to run a Factor Server, you will want to use a process manager to run the server in production. The process manager ensures that Factor.io will continue to run even in case of an unexepcted crash or system reboot.

## Running on Ubuntu with Upstart
Upstart in Ubuntu is a great tool which will run the Factor.io server as well as restarting it when the server restarts.

To take advantage of this place this contents in `/etc/init/factor`. You will need sudo access.

    pre-start script
      mkdir -p /var/log/factor
    end

    start on runlevel [2345]
    stop on runlevel [016]
    respawn

    exec bundle exec foreman start --credentials=~/factor/credentials.yml --connectors=~/factor/connectors.yml --path=~/factor >> /var/log/factor.log

When you restart your server, Factor.io will start automatically. However, to start the server now, just run this...

    sudo start factor


## Running from the command line
The [Foreman gem](https://github.com/ddollar/foreman) is a generic process manager which uses a Procfile to run and manage multiple process. To start foreman just run...

    cd example-workflows
    foreman start

## Running on Heroku
Heroku uses Foreman and Procfiles under the hood, so other than having the Procfile in your directory there isn't much you need to do.

    cd example-workflows
    heroku create
    git push heroku
