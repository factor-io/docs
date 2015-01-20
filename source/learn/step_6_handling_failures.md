---
title: "Step 6: Handling failures"
category: Learn Factor.io
order: 6
---
Shit happens! And when it does, we need a way to handle errors/exceptions in our workflows.

There are two main techniques for handling failure scenarios.

# Using on_fail
If you look at the blog below, you will notice that after the `run ... do |repo ... end` block, there is another block which starts with `on_fail`.

This is a mechanism you can use when the connector either has an unhandled exception, or in intentionally failed. Most of the connectors are implemented to fail when validation fails, like a missing parameter, or a call to the third party service fails. In those cases the block after `on_fail` will be called.

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

# Using the results
Sometimes an exception or failure isn't raised by the connector. The actually operation might have succeeded, but the results are not favorible. For example ...

```ruby
run 'jenkins::jobs::build', job:'factor-test' do |results|
  if results.status == 'success'
    # do more work...
  else
    # the test ran, but the test itself failed.
  end
end
```

In this case, we don't use the `on_fail` block, but rather, we use the output of the operation to check the status. It is up to the connector developer to provide this additional information, and it should be documented in the connector docs.