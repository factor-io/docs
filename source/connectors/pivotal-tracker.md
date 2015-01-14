---
title: PivotalTracker
category: Connectors
status: incomplete
logo: https://raw.githubusercontent.com/factor-io/connector-pivotal/master/logo.png
---

# Authentication
To use the Pivotal Tracker connector you will need to obtain an API Key from Pivotal Tracker.

The API Key can be found at [https://www.pivotaltracker.com/profile](https://www.pivotaltracker.com/profile) in the API Token section. Copy and paste this key into your connectors.yml file.

```yaml
---
pivotal:
  api_key: 062070f3b32a86035a6ac27b1642d54f
```
# Projects

### pivotal::projects::all


### Parameters
This doesn't require any parameters

### Output


### pivotal::projects::get

### Parameters

ID | Default | Description
--- | ------- | -----------
id |  | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.

# Stories

### pivotal::stories::all

### Parameters

ID | Default | Description
--- | ------- | -----------
project |  | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.

### pivotal::stories::get

### Parameters

ID | Default | Description
--- | ------- | -----------
project |  | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
id |   | The story ID, which can be found in the story on the site when editing.

### pivotal::stories::create

### Parameters

ID | Default | Description
--- | ------- | -----------
project |  | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
name | | The name of the story.
description | (optional) | 
story_type | Feature | feature, bug, chore, or release.
estimate | (optional) |
current_state | unstarted | unstarted, started, finished, delivered, rejected, accepted
requested_by | (optional) | Name of requester
owned_by | (optional) | Name of owner
labels | (optional) | a common seperated list of labels
jira_id | (optional) | ID of the related Jira issue
jira_url | (optional) | URL of the related Jira issue
other_id | (optional) | 
zendesk_id | (optional) | ID of the related Zendesk issue
zendesk_url | (optional) | URL to the related Zendesk issue
integration_id | (optional) | 
deadline | (optional) | 

### pivotal::stories::update
### pivotal::stories::delete

# Notes

### pivotal::notes::get
### pivotal::notes::all
### pivotal::notes::create



