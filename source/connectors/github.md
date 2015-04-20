---
title: Github
category: Connectors
connector_type: Source Code Management
logo: https://raw.githubusercontent.com/factor-io/connector-github/master/logo.png
---
# Authentication
Github requires an API Key (api_key) to be defined in your credentials.yml file. To get the API key go to your [Application Settings page in Github](https://github.com/settings/applications) and use the "Generate new token" button in the Personal access tokens section to generate a new token. Copy the new key and paste it into your credentials.yml file.

### credentials.yml

```yaml
github:
  api_key: 4686b1488574a768a8a8d
```

# Repositories

## Commit Commet Listener
### github::repo::commit_comment
The Commit Comment Listener (`github::repo::commit_comment`) is triggered when a commit comment is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#commitcommentevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::commit_comment', repo:'skierkowski/hello' do |comment|
  info "#{comment.user.login} just commented '#{comment.body}' on #{comment.repository.full_name}"
end
```

## Create Listener
### github::repo::create
The Create Listener (`github::repo::create`) listeners for the creation of new branches, or tags. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#createevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::create', repo:'skierkowski/hello' do |create_event|
  if create_event.ref_type == 'tag'
    info "#{create_event.sender.login} just created a new tag called #{create_event.ref}"
  elsif create_event.ref_type == 'branch'
    info "#{create_event.sender.login} just created a new branch called #{create_event.ref}"
  end
end
```

## Delete Listener
### github::repo::delete
The Delete Listener (`github::repo::delete`) listeners for the deleting of new branches, or tags. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deleteevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::delete', repo:'skierkowski/hello' do |delete_event|
  if delete_event.ref_type == 'tag'
    info "#{delete_event.sender.login} just deleted a new tag called #{delete_event.ref}"
  elsif delete_event.ref_type == 'branch'
    info "#{delete_event.sender.login} just deleted a new branch called #{delete_event.ref}"
  end
end
```

## Deployment Listener
### github::repo::deployment
The Deployment Listener (`github::repo::deployment`) is triggered on new deployments. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deployevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::deployment', repo:'skierkowski/hello' do |deploy|
  info "The #{deploy.name}/#{deploy.ref} is getting deployed to the #{deploy.environment} environment."
end
```

## Deployment Status Listener
### github::repo::deployment_status
The Deployment Status Listener (`github::repo::deployment_status`) is triggered when the status of a deployment changes. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deploymentstatusevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::deployment_status', repo:'skierkowski/hello' do |deploy|
  info "The #{deploy.name}/#{deploy.ref} deployment status changed to #{deploy.status} for the deployment to the #{deploy.deployment.environment} environment"
end
```
## Download Listener
### github::repo::download
The Download Listener (`github::repo::download`) is triggered when a download is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#downloadevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::download', repo:'skierkowski/hello'
  success "A new download was created"
end
```

## Fork Listener
### github::repo::fork
The Fork Listener (`github::repo::fork`) is triggered when the repo is forked. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#forkevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::fork', repo:'skierkowski/hello' do |fork|
  info "#{fork.forkee.name} just forked #{fork.repository.full_name}"
end
```

## Gollum Listener
### github::repo::gollum
The Gollum Listener (`github::repo::gollum`) is triggered when a Wiki page is created or updated. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#gollumevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example

```ruby
listen 'github::gollum', repo:'skierkowski/hello' do |wiki|
  info "#{wiki.pages.count} pages have been created or updated in #{wiki.repository.full_name} by #{wiki.sender.login}"
end
```

## Comment Listener
### github::repo::issue_comment
The Issue Comment Listener (`github::repo::issue_comment`) is triggered when an issue comment is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#issuecommentevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::issue_comment', repo:'skierkowski/hello' do |comment|
  info "#{comment.sender.login} #{comment.action} a comment on issue #{comment.issue.title} with `#{comment.comment.body}`"
end
```

## Issue Listener
### github::repo::issues
The Issues Listener (`github::repo::issues`) is triggered when an issue is assigned, unassigned, labeled, unlabeled, opened, closed, or reoponed. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#issuesevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::issues', repo:'skierkowski/hello' do |issue|
  info "#{issue.sender.login} #{issue.action} an issue titled #{issue.title}"
end
```

## Member Listenr
### github::repo::member
The Member Listener (`github::repo::member`) is triggered when a user is added as a collaborator to a repository. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#memberevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::member', repo:'skierkowski/hello' do |issue|
  info "#{member.login} was #{action} to the repo #{issue.repository.full_name}"
end
```

## Page Build Listener
### github::repo::page_build
The Page Build Listener (`github::repo::page_build`) is triggered when an attempt is made to build a Github Page site, whether successful or not. This is triggered on push to a Github Pages enabled branch, either `gh-pages` for project pages, or `master` for user/organization pages. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pagebuildevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::page_build', repo:'skierkowski/hello' do |page|
  if page.build.status == 'built'
    success "#{page.build.pusher.login} successfully built #{page.repository.full_name}"
  else
    fail "Failed to build #{page.repository.full_name}"
  end  
end
```

## Public Flag Listener
### github::repo::public
The Public Listener (`github::repo::public`) is triggered when a repo is open sourced. We need more of these. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#publicevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::public', repo: 'factor-io/factor' do |repo|
  success "Yey, we went open source!"
end
```

## Pull Request Listener
### github::repo::pull_request
The Pull Request Listener (`github::repo::pull_request`) is triggered when a pull request is assigned, unassigned, labeled, unlabeled, opened, closed, reopened, or synchronized. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pullrequestevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::pull_request', repo:'skierkowski/hello' do |request|
  msg = "Pull request #{request.number} was #{request.action}: #{request.pull_request.title}"
  run 'hipchat::send', room:'Engineering', message: msg
end
```

## Pull Request Review Comment Listener
### github::repo::pull_request_review_comment
The Pull Request Review Comment Listener (`github::repo::pull_request_review_comment`) is triggered when a comment is created on a portion of the unified diff of a pull request. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pullrequestreviewcommentevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::pull_request_review_comment', repo:'skierkowski/hello' do |comment|
  info "#{comment.sender.login} #{comment.action} commented #{comment.comment.body} on #{comment.repository.full_name}"
end
```

## Push Listener
### github::repo::push
The Push Listener (`github::repo::push`) is triggered when a repository branch is pushed to. This is the most used listener as it is often used to invoke deployments when code is pushed or merged into the master branch. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pushevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
  run 'heroku::deploy', app:'hello-frank', content:github_info.content do |deploy_info|
    info 'Deployment is complete'
  end
end
```

## Release Listener
### github::repo::release
The Release Listener (`github::repo::release`) is triggered when a release is published. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#releaseevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::release', repo:'skierkowski/hello#master' do |release|
  info "Release #{release.release.tag_name} was #{release.action}"
end
```
## Status Listener
### github::repo::status
The Status Listener (`github::repo::status`) is triggered when the status of a git commit changes. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#statusevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::status', repo:'skierkowski/hello' do |status|
  info "Commit #{status.commit.message} was a #{status.state}"
end
```

## Team Add Listener
### github::repo::team_add
The Team Add listener (`github::repo::team_add`) is triggered when a user or repository is added to a team. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#teamaddevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::team_add', repo:'skierkowski/hello' do |add_info|
  if add_info.team
    info "Team #{add_info.team.name} was added to #{add_info.repository.full_name}"
  elsif add_info.user
  info "Team #{add_info.user.login} was added to #{add_info.repository.full_name}"
  end
end
```

## Watch Listener
### github::repo::watch
The Watch Listener (`github::repo::watch`) is triggered when a user Stars a repository (not watches). The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#watchevent).

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

### Example
```ruby
listen 'github::watch', repo:'skierkowski/hello' do |watch|
  info "#{watch.sender.login} #{watch.action} repo #{watch.repository.full_name}"
end
```

# Deployments
Deployments are a request for a specific ref(branch,sha,tag) to be deployed. GitHub then dispatches deployment events that external services can listen for and act on. This enables developers and organizations to build loosely-coupled tooling around deployments, without having to worry about implementation details of delivering different types of applications (e.g., web, native).

Deployment Statuses allow external services to mark deployments with a ‘success’, ‘failure’, ‘error’, or ‘pending’ state, which can then be consumed by any system listening for deployment_status events.

Deployment Statuses can also include an optional description and target_url, and we highly recommend providing them as they make deployment statuses much more useful. The target_url would be the full URL to the deployment output, and the description would be the high level summary of what happened with the deployment.

Deployments and Deployment Statuses both have associated repository events when they’re created. This allows webhooks and 3rd party integrations to respond to deployment requests as well as update the status of a deployment as progress is made.

## List Deployments Action
### github::deployments::list

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

## Create a Deployment Action
### github::deployments::create

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
ref | (required) | The ref parameter can be any named branch, tag, or sha.
auto_merge | true | Optional parameter to merge the default branch into the requested ref if it is behind the default branch. The auto_merge parameter is used to ensure that the requested ref is not behind the repository’s default branch. If the ref is behind the default branch for the repository, we will attempt to merge it for you. If the merge succeeds, the API will return a successful merge commit. If merge conflicts prevent the merge from succeeding, the API will return a failure response.
task | "deploy" | Optional parameter to specify a task to execute, e.g. deploy or deploy:migrations. The task parameter is used by the deployment system to allow different execution paths. In the web world this might be ‘deploy:migrations’ to run schema changes on the system. In the compiled world this could be a flag to compile an application with debugging enabled.
required_contexts | | Optional array of status contexts verified against commit status checks. If this parameter is omitted from the parameters then all unique contexts will be verified before a deployment is created. To bypass checking entirely pass an empty array. Defaults to all unique contexts. By default, commit statuses for every submitted context must be in a ‘success’ state. The required_contexts parameter allows you to specify a subset of contexts that must be “success”, or to specify contexts that have not yet been submitted. You are not required to use commit statuses to deploy. If you do not require any contexts or create any commit statuses, the deployment will always succeed.
payload | "" | The payload parameter is available for any extra information that a deployment system might need. It is a JSON text field that will be passed on when a deployment event is dispatched.
environment | "production" | The environment parameter allows deployments to be issued to different runtime environments. Teams often have multiple environments for verifying their applications, like ‘production’, ‘staging’, and ‘qa’. This allows for easy tracking of which environments had deployments requested.
description | "" | Optional short description.


## List Deployment Statuses Action
### github::deployments::statuses

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
id | (required) | The Deployment ID.

## Create Deployment Status Action
### github::deployments::create_status

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
id | (required) | The deployment ID.
state | | The state of the status. Can be one of pending, success, error, or failure.
target_url | "" | he target URL to associate with this status. This URL should contain output to keep the user updated while the task is running or serve as historical information for what happened in the deployment.
description | "" | A short description of the status.

# Issues

## List Issues Action
### github::issues::list
List all issues across all the authenticated user’s visible repositories including owned repositories, member repositories, and organization repositories.

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
filter | assigned | Indicates which sorts of issues to return. Can be assigned, created, mentioned, subscribed, or all.
state | open | Indicates the state of the issues to return. Can be either open, closed, or all
since | | Only issues updated at or after this time are returned. This is a timestamp in ISO 8601 format: YYYY-MM-DDTHH:MM:SSZ.
labels | | A list of comma separated label names. Example: bug,ui,@high
sort | created | What to sort results by. Can be either created, updated, comments. 
direction | desc | The direction of the sort. Can be either asc or desc.

## Get a Single Issue Action
### github::issues::get

**Note**: Every pull request is an issue, but not every issue is a pull request. If the issue is not a pull request, the response omits the pull_request attribute.

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
number | (required) | The issue number

## Create an Issue Action
### github::issues::create

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
title | (required) | The title of the issue.
body | | The contents of the issue.
assignee | | Login for the user that this issue should be assigned to. **NOTE**: Only users with push access can set the assignee for new issues. The assignee is silently dropped otherwise.
milestone | | Milestone to associate this issue with. **NOTE**: Only users with push access can set the milestone for new issues. The milestone is silently dropped otherwise.
labels | | Labels to associate with this issue. Array of strings. **NOTE**: Only users with push access can set labels for new issues. Labels are silently dropped otherwise.

## Edit Issue Action
### github::issues::edit

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
number | (required) | The issue number
title | (required) | The title of the issue.
body | | The contents of the issue.
assignee | | Login for the user that this issue should be assigned to. **NOTE**: Only users with push access can set the assignee for new issues. The assignee is silently dropped otherwise.
milestone | | Milestone to associate this issue with. **NOTE**: Only users with push access can set the milestone for new issues. The milestone is silently dropped otherwise.
labels | | Labels to associate with this issue. Array of strings. **NOTE**: Only users with push access can set labels for new issues. Labels are silently dropped otherwise.

## Close Issue Action
### github::issues::close

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
number | (required) | The issue number

# Releases

## List Releases Action
### github::releases::list
Information about published releases are available to everyone. Only users with push access will receive listings for draft releases.

**Note**: This returns a list of releases, which does not include regular Git tags that have not been associated with a release.

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.

## Get Release Action
### github::releases::get

**Note**: This returns an upload_url key corresponding to the endpoint for uploading release assets.

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
id | (required) | Release ID

## Create Release Action
### github::releases::create

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
tag_name | (required) | The name of the tag. This can be an existing tag, or a new tag will be created.
target_commitish | (the default branch, usually 'master') | Specifies the commitish value that determines where the Git tag is created from. Can be any branch or commit SHA. Unused if the Git tag already exists.
name | | The name of the release
body | | Text describing the contents of the tag.
draft | false | true to create a draft (unpublished) release, false to create a published one. 
prerelease | false | true to identify the release as a prerelease. false to identify the release as a full release. 

## Delete Relase Action
### github::releases::delete

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
id | (required) | Release ID

# Statuses
The Status API allows external services to mark commits with a success, failure, error, or pending state, which is then reflected in pull requests involving those commits.

Statuses can also include an optional description and target_url, and we highly recommend providing them as they make statuses much more useful in the GitHub UI.

As an example, you can mark a commit as 'passing' or 'failing' after getting the status from Jenkins. The target_url would be the full URL to the build output, and description would be the high level summary of the job status.

Statuses can include a context to indicate what service is providing that status (e.g. you may use `factor`). You can then use the combined status endpoint to retrieve the whole status for a commit.

## List Statuses Action
### github::statuses::list

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
ref | (required) | Ref to list the statuses from. It can be a SHA, a branch name, or a tag name.

## Create Statuses Action
### github::statuses::create

### Parameters
ID | Default | Description
--- | --- | ---
username | (required) | The username of the account where your repo is hosted. For example http://github.com/skierkowski/hello has the username 'skierkowski'.
repo | (required) | The repository you'd like to use with this action. For example http://github.com/skierkowski/hello has the repo 'repo'.
state | (required) | The state of the status. Can be one of pending, success, error, or failure.
target_url | | The target URL to associate with this status. This URL will be linked from the GitHub UI to allow users to easily see the ‘source’ of the Status. For example, if your Continuous Integration system is posting build status, you would want to provide the deep link for the build output for this specific SHA: http://ci.example.com/user/repo/build/sha.
description | | A short description of the status.
context | "default" | A string label to differentiate this status from the status of other systems.
