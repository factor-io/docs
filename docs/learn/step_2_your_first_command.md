Let's create our working directory, `workflows`, and we'll add one file `connectors.yml`.
```shell
mkdir ~/workflows
cd ~/workflows
touch connectors.yml
```

## Configure connectors.yml
Let's configure our Connectors.

**connectors.yml**
```yaml
---
web: wss://open-connectors.factor.io/v0.4/web
```
This is a YAML-formatted file which consists of a hash where the key is the service name and the leaves are the URIs to the Connectors.

A **Connector** is a web service which acts as a common API to all third party services. The `factor run` and `factor server` commands (which we will use) depend on the connector service to integrate with third party services. You can run your own connector service and even build your own integrations. In these examples we will use an open and public connector service provided by Factor.io at `open-connectors.factor.io`.

## Run your first command
First, go to [http://requestb.in/](http://requestb.in/) and `Create a RequestBin`. Replace the URL value with the one provided by RequestBin.

Now run this command
```shell
factor run web::post '{"url":"http://requestb.in/1h15g3i1","params":{"foo":"bar"}}'
```

When you run this, you should see output like this.

```shell
[    INFO    ] [11/20/14 13:59:16.417] Action 'web::post' starting
[    INFO    ] [11/20/14 13:59:16.533]   Posting to `http://requestb.in/1h15g3i1`
[  SUCCESS   ] [11/20/14 13:59:16.577] Action 'web::post' responded
[    INFO    ] [11/20/14 13:59:16.577] {
[    INFO    ] [11/20/14 13:59:16.577]   "response": "ok"
[    INFO    ] [11/20/14 13:59:16.577] }
[    INFO    ] [11/20/14 13:59:16.578] Good bye!
```

The first option, `web::post` instructed the runtime to call the action `post` on the `web` connector. The runtime opened up the `connectors.yml` file and found the URL by looking up the value for `web`. Based on the URL it connected to the service to execute the `post` action.

Now when you visit your PostBin URL you will see that `foo: bar` was posted as a Form/Post parameter.

## Nesting connectors.yml
Sometimes for the sake of organization we may want to nest our `connectors.yml` file.

Suppose we modify our `connectors.yml` file to this..

**connectors.yml**
```yaml
---
myconnector:
  webstuff: wss://open-connectors.factor.io/v0.4/web
```

We can then perform the exact same action by calling

```shell
factor run myconnector::webstuff::post '{"url":"http://requestb.in/1h15g3i1"}'
```


### [Next: Your first workflow](learn/step_3_your_first_workflow)
#### [Previous: Before you start](learn/step_1_before_you_start)