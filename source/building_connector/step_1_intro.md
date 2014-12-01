---
title: "Step 1: Intro"
category: Build a custom connector
order: 1
---

From the [Learn Factor.io tutorial](/learn/step_1_before_you_start.html) you learned how to use `factor s` and `factor run`. To understand what happened under the hood we need to walk through the end-to-end process.

1. When you run `factor run` or `factor s`, the **Factor.io Runtime** was loaded, which in hand loaded the workflows, connectors.yml, and credentials.yml file. The Runtime then connected to the Connector Service.
2. The **Connector Service** is a Sinatra-based web app. It provides a web address (e.g. /v0.4/github) for each of the supported integrations. Each of those integrations is called a Connector.
3. The **Connector** is an independent ruby gem which implements the integration with the third party service.
4. Both the Connector Service and Connectors use the **[Connector API](https://github.com/factor-io/connector-api)** to provide a common API for the Connector Service to know how to load each of the Connectors.

In this tutorial we'll learn how to implement a **Connector**, load it into the Connector Service, call it from the Runtime, and write RSpec tests.
