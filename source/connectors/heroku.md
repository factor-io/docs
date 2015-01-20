---
title: Heroku
category: Connectors
connector_type: Hosting
logo: https://factor.io/assets/channel_logos/heroku.png
---
# Authentication
Github requires an API Key (api_key) to be defined in your credentials.yml file. To get the API key go to your [Account Page in Heroku](https://dashboard.heroku.com/account) and generate an "API Key". Copy the new key and paste it into your credentials.yml file.

**credentials.yml**

```yaml
heroku:
  api_key: 4686b1488574a768a8a8d
```


# heroku::deploy
The Deploy Action (`heroku::deploy`) is an action to deploy files to an application in Heroku.

## Parameters

ID | Default | Description
--- | --- | ---
app | | The Heroku application ID you are deploying.
content | | The reference to the contents that is to be deployed. The contents is retreived from anther listener (e.g. github::push) or action (e.g. github::download)


## Output prameters

```ruby
{
  "created_at": "2014-11-25T04:44:13Z",
  "description": "Deploy",
  "id": "48ce9058-ce59-490c-b27b-ad4290617129",
  "slug": "",
  "updated_at": "2014-11-25T04:44:13Z",
  "user": "",
  "version": 389
}
```

## Example
```ruby
listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
  run 'heroku::deploy', app:'hello-frank', content:github_info.content do |deploy|
    info "Deployment completed at #{deploy.created_at}, version: #{deploy.version}"
  end
end
```