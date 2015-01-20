---
title: Pivotal Tracker
category: Connectors
connector_type: Project Management
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

## Get all Projects
### pivotal::projects::all
Gets a list of all the project to which you have access.

### Parameters
This doesn't require any parameters.

### Output
Returns an Array of [Project](https://www.pivotaltracker.com/help/api/rest/v5#project_resource)s as a hash.

## Get a Project
### pivotal::projects::get
Gets a specifc project to which you have access.

### Parameters

ID | Default | Description
--- | ------- | -----------
id | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.

### Output
Returns a [Project](https://www.pivotaltracker.com/help/api/rest/v5#project_resource) as a Hash.

# Stories

## Get all Stories
### pivotal::stories::all
Gets a list of all the stories for a given project

### Parameters

ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.

### Output
An Array of [Stories](https://www.pivotaltracker.com/help/api/rest/v5#story_resource) represented as a Hash.

## Get a Story
### pivotal::stories::get
Get a story by it's Project and Story IDs.

### Parameters

ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
id | (required) | The story ID, which can be found in the story on the site when editing.

### Output
Returns a [Story](https://www.pivotaltracker.com/help/api/rest/v5#story_resource) as a Hash.

## Create a new Story
### pivotal::stories::create
Creates a new story.

### Parameters

ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
name | (required) | The name of the story.
description | (optional) | In-depth explanation of the story requirements.
story_type | Feature | feature, bug, chore, or release.
estimate | (optional) | Point value of the story.
current_state | unstarted | unstarted, started, finished, delivered, rejected, accepted
accepted_at | (optional) | Acceptance time
requested_by_id | (optional) | ID of user who requested story
owner_ids | (optional) | IDs of the current story owners.
follower_ids | (optional) | IDs of people currently following the story.
created_at | (optional) | Creation time. This field is writable only on create.
external_id | (optional) | The integration's specific ID for the story. (Note that this attribute does not indicate an association to another resource.)
integration_id | (optional) | ID of the integration API that is linked to this story. In API responses, this attribute may be integration_id or integration.
deadline | (optional) | Due date/time (for a release-type story).

### Output
Returns the [Story](https://www.pivotaltracker.com/help/api/rest/v5#story_resource) as a Hash.

## Update a Story
### pivotal::stories::update
Update a story.

### Parameters

ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
name | (required) | The name of the story.
description | (optional) | In-depth explanation of the story requirements.
story_type | Feature | feature, bug, chore, or release.
estimate | (optional) | Point value of the story.
current_state | unstarted | unstarted, started, finished, delivered, rejected, accepted
accepted_at | (optional) | Acceptance time
requested_by_id | (optional) | ID of user who requested story
owner_ids | (optional) | IDs of the current story owners.
follower_ids | (optional) | IDs of people currently following the story.
external_id | (optional) | The integration's specific ID for the story. (Note that this attribute does not indicate an association to another resource.)
integration_id | (optional) | ID of the integration API that is linked to this story. In API responses, this attribute may be integration_id or integration.
deadline | (optional) | Due date/time (for a release-type story).

## Delete a Story
### pivotal::stories::delete
Delete a story.

### Parameters

ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
id | (required)  | The story ID, which can be found in the story on the site when editing.

### Output
Nothing. Once deleted this returns null.

# Comments

## Get a Comment
### pivotal::comments::get
Get a comment for a given story in a project

### Parameters
ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story | (required) | The story ID, which can be found in the story on the site when editing.
id | (required) | The Comment ID

### Output
Returns a [Comment](https://www.pivotaltracker.com/help/api/rest/v5#comment_resource) as a Hash.

## Get all Comments
### pivotal::comments::all
Get all the comments for a Story.

### Parameters

ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story |  (required) | The story ID, which can be found in the story on the site when editing.

### Output
Returns an Array of [Comment](https://www.pivotaltracker.com/help/api/rest/v5#comment_resource)s as a Hashes.

## Create a Comment
### pivotal::comments::create
Creates a new comment

### Parameters
ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story |  (required) | The story ID, which can be found in the story on the site when editing.
text | (required) | The long text to add as a comment
person_id | (optional) | ID of the person.
commit_identifier | (optional) | Commit Id on the remote source control system for the comment. Present only on comments that were created by a POST to the source commits API endpoint. This field is writable only on create.
commit_type | (optional) | String identifying the type of remote source control system if Pivotal Tracker can determine it. Present only on comments that were created by a POST to the source commits API endpoint. This field is writable only on create.

### Output
Returns a [Comment](https://www.pivotaltracker.com/help/api/rest/v5#comment_resource) as a Hash.

## Delete a Comment
### pivotal::comments::delete
Delete a comment
### Parameters
ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story |  (required) | The story ID, which can be found in the story on the site when editing.
id | (required) | Comment ID

### Output
Nothing. Once deleted this is null.

# Labels

## Create a Label

### pivotal::labels::create
### Parameters
ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story |  (required) | The story ID, which can be found in the story on the site when editing.
name | (required) | A string with no white spaces to use as a label.


### Output
A [Label](https://www.pivotaltracker.com/help/api/rest/v5#label_resource) as a Hash.

## Delete a Label
### pivotal::labels::delete
### Parameters
ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story |  (required) | The story ID, which can be found in the story on the site when editing.
id | (required) | Label ID
### Output
Nothing.

## Get all Labels
### pivotal::lables::all
### Parameters
ID | Default | Description
--- | ------- | -----------
project | (required) | The Project ID can be found in the URL. For example, the project ID is `1244700` for https://www.pivotaltracker.com/n/projects/1244700. **Note**: this must be an integer.
story |  (required) | The story ID, which can be found in the story on the site when editing.
### Output
An Array of [Label](https://www.pivotaltracker.com/help/api/rest/v5#label_resource)s as a Hash.
