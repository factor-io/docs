[BitBalloon](https://www.bitballoon.com/) is a static site hosting service. Factor.io provides a way to deploy static files to your site hosted on BitBalloon.

# Authentication
BitBalloon only requires an API Key (api_key) for the credentials. First, visit the [Applications and Tokens](https://www.bitballoon.com/applications) page. Under "Personal access token" generate a new personal token. Grab the "Access token" and add it to your credentials.yml file in your working directory.

**credentials.yml**

    bitballoon:
      api_key: 4686b1488574a768a8a8d


# bitballoon::deploy
The deploy action (`bitballoon::deploy`) provides the means of uploading your full static site to BitBalloon.


## Parameters

- **site** (required): This is the GUID of your site. When you deploy your application from the command line, the BitBalloon CLI will generate a .bitballoon file in that directory. You can find the site ID in that file.
- **content** (requried): This is a reference to the files to be uploaded. Other actions like github::push, github::download, ssh::download will generate a reference to these files.


## Example
This example will wait for a git push in github, then upload it, build it, and download it from an SSH server. Once complete, it 

```ruby
listen 'github::push', repo:'skierkowski/hello' do |repo_info|
  host = 'ubuntu@sandbox.factor.io'
  run 'ssh::upload', content:repo_info.content, path:'~/web', host:host
    run 'ssh::execute', commands:['middleman build ~/web'], host:host do |build_info|
      run 'ssh::download', path:'~/web/build', host:host do |download_info|
        run 'bitballoon::deploy', site:'foo.com', content:download_info.content
      end
    end
  end
end
```