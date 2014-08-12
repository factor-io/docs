# Heroku

## Authentication
Github requires an API Key (api_key) to be defined in your credentials.yml file. To get the API key go to your [Account Page in Heroku](https://dashboard.heroku.com/account) and generate an "API Key". Copy the new key and paste it into your credentials.yml file.

**credentials.yml**

    heroku:
      api_key: 4686b1488574a768a8a8d


## Deploy action
The Deploy Action (`heroku::deploy`) is an action to deploy files to an application in Heroku.

### Parameters
- **app**: The Heroku application ID you are deploying.
- **content**: The reference to the contents that is to be deployed. The contents is retreived from anther listener (e.g. github::push) or action (e.g. github::download)

### Output prameters
- **release**: Release number used to track in Heroku

### Example
    listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
      run 'heroku::deploy', app:'hello-frank', content:github_info.content do |deploy_info|
        info 'Deployment is complete'
      end
    end