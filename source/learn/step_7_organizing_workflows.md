---
title: "Step 7: Organizing workflows"
category: Learn Factor.io
order: 7
---

Once you start creating more complex orchestrations you'll notice that the nesting becomes difficult to read. As such, we provide a way to package repeatable workflow sequences into their own `workflow` blocks. 

```ruby
workflow 'bootstrap' do |options|
  run 'ssh::upload', host:options.host, content:options.content, path:'/var/factor/app'
    run 'ssh::execute', host:options.host, commands: ['./deploy']
  end
end

listen 'github::push', repo:'skierkowski/hello' do |gh|
  run 'workflow::bootstrap', host:'root@sandbox.factor.io', content:gh.content
end
```

Using the above syntax you can wrap a sequence of work in a `workflow` block. You can call this block with `run` command by addressing it with `workflow::(workflow name)`.

