---
title: Jenkins
category: Connectors
connector_type: Continuous Integration
logo: https://raw.githubusercontent.com/factor-io/connector-jenkins/master/logo.png
---
# Authentication

You must provide the username, password, and host to access the Jenkins server using a fully qualified domain name.

**credentials.yml**

```yaml
---
jenkins:
  host: http://skierkowski:password@jenkins.factor.io
```


# Jobs

## List all jobs
### jenkins::job::list

### Input

ID | Default | Description
--- | ------- | -----------
filter | (optional) | Regex for searching through jobs
status | (optional) | The job status to filter by

**Note**: You can provide the filter, status, or neither, but you cannot provide both filter and status. 

## Build a Job
### jenkins::job::build
This will build an existing job and wait until the build is complete (success or failure).

### Input

ID | Default | Description
--- | ------- | -----------
job |  | The Job ID to build

### Output
ID | Description
--- | ---
build_number | The Build ID
status | Status of the build after the build complete
console[n].output | Text output of the console for the build
console[n].size | The size of the console output
console[n].more | Link to more content


## Get a Job Status
### jenkins::job::status
The job status returns the build status of the last build.

### Input

ID | Default | Description
--- | ------- | -----------
job |  | The Job ID to get the status

### Output

ID | Description
--- | ---
status | The string value of the last build status (e.g. 'success')
