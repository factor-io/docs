---
title: Jenkins
category: Connectors
connector_type: Continuous Integration
status: incomplete
logo: https://factor.io/assets/channel_logos/jenkins.png
---
# Authentication

You must provide the username, password, and host to access the Jenkins server

## credentials.yml
```yaml
---
jenkins:
  username: skierkowski
  password: password
  host: jenkins.factor.io
```

## connectors.yml
```yaml
---
jenkins:
  jobs: wss://open-connectors.factor.io/v0.4/jenkins_job
```

# Jobs

## List Jobs Action
### jenkins::job::list

### Input

ID | Default | Description
--- | ------- | -----------
filter | (optional) | Regex for searching through jobs
status | (optional) | The job status to filter by

**Note**: You can provide the filter, status, or neither, but you cannot provide both filter and status. 

## Build Job Action
### jenkins::job::build

### Input

ID | Default | Description
--- | ------- | -----------
job |  | The Job ID to build

### Output
```javascript
{
  code: '200'
}
```

## Stop Job Action
### jenkins::job::stop

### Input

ID | Default | Description
--- | ------- | -----------
job |  | The Job ID to stop

### Output
```javascript
{
  code: '200'
}
```

## Get Job Status Action
### jenkins::job::status

### Input

ID | Default | Description
--- | ------- | -----------
job |  | The Job ID to get the status

### Output
```javascript
{
  code: '200'
}
```

## Rename Job Action
### jenkins::job::rename

### Input

ID | Default | Description
--- | ------- | -----------
job |  | The Job ID to rename
new_job | | The new Job ID

### Output
```javascript
{
  code: '200'
}
```