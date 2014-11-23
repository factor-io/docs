# Step 5: Automate deployments

```ruby
listen 'github::push', repo:'skierkowski/hello' do |repo|
  run 'heroku::push', app:'hello-frank', app:repo.content do |deploy|
    success "Deploy complete"
  end
end
```

### [Next: Handling failures](learn/step_6_handling_failures)
#### [Previous: Getting more services](learn/step_4_getting_more_services)