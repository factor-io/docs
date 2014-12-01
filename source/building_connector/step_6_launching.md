---
title: "Step 6: Official Launch"
category: Build a custom connector
order: 6
---

If your connector is deemed "official", it will become available through the command `factor registry connectors list` and `factor registry connectors add` so users can easily add it to their projects. Before your connector is made official you need to implement a few additional small things.

## Documentation
Your new connector should be well documented. All public connectors have a section on the [official Docs site at http://docs.factor.io](http://docs.factor.io/connectors/overview.html).

The source code for the docs is open. You can find it at [https://github.com/factor-io/docs](https://github.com/factor-io/docs).


## Adding to Connector Service
You can create a Pull Request for [https://github.com/factor-io/connector](https://github.com/factor-io/connector) to the `Gemfile`, `Gemfile.lock`, and `init.rb` files to add integration for your gem. Any time you update your gem thereafter, you'll need to create a new pull request for this repo to update the Gemfile.lock file.

## Adding to Index
The commands `factor registry connectors list` and `factor registry connectors add` use the index provided at [https://github.com/factor-io/index](https://github.com/factor-io/index) to get the metadata. You need to create a pull request updating the connectors.yml file.
