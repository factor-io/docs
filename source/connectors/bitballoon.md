---
title: BitBalloon
category: Connectors
connector_type: Hosting
logo: https://factor.io/assets/channel_logos/bitballoon.png
---
[BitBalloon](https://www.bitballoon.com/) is a static site hosting service. Factor.io provides a way to deploy static files to your site hosted on BitBalloon.

# Authentication
BitBalloon only requires an API Key (api_key) for the credentials. First, visit the [Applications and Tokens](https://www.bitballoon.com/applications) page. Under "Personal access token" generate a new personal token. Grab the "Access token" and add it to your credentials.yml file in your working directory.

**credentials.yml**

```yaml
---
bitballoon:
  api_key: 4686b1488574a768a8a8d
```

# Deploy
## bitballoon::deploy
The deploy action (`bitballoon::deploy`) provides the means of uploading your full static site to BitBalloon.


## Parameters

ID | Default | Description
--- | --- | ---
site | (required) | This is the GUID of your site. When you deploy your application from the command line, the BitBalloon CLI will generate a .bitballoon file in that directory. You can find the site ID in that file.
content | (required) | This is a reference to the files to be uploaded. Other actions like github::push, github::download, ssh::download will generate a reference to these files.

## Example
This example will wait for a `git push` in Github, then upload it to BitBaloon. 

```ruby
listen 'github::push', repo:'skierkowski/hello' do |repo|
  run 'bitballoon::deploy', site:'foo.com', content:repo.content do |deploy|
    success "Bitballoon finished deploying"
  end
end
```