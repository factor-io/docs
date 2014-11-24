
```ruby
listen 'github::push', repo:'skierkowski/hello' do |repo|
  run 'heroku::push', app:'hello-frank', app:repo.content do |deploy|
    success "Deploy complete"
  end.on_fail do
    error 'Heroku failed to deploy the application'
    run 'hipchat::send', room:'Factor', message:"Heroku failed to deploy the app 'hello-frank'"
  end
end
```


### [Next: Organizing Workflows](learn/step_7_organizing_workflows)
#### [Previous: Automating deployments](learn/step_5_auto_deploy)