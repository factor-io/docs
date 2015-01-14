---
title: Github
category: Connectors
logo: https://factor.io/assets/channel_logos/github.png
---
# Authentication
Github requires an API Key (api_key) to be defined in your credentials.yml file. To get the API key go to your [Application Settings page in Github](https://github.com/settings/applications) and use the "Generate new token" button in the Personal access tokens section to generate a new token. Copy the new key and paste it into your credentials.yml file.

### credentials.yml

```yaml
github:
  api_key: 4686b1488574a768a8a8d
```

# Parameters
All of the listeners below require the same set of parameters

ID | Default | Description
--- | --- | ---
repo | | The address of the repo in the format (user/repo#branch). The use rand repo in the format are required while the branch is optional. If the branch is not specified it will default to `master`. For example `skierkowski/hello` would refer to https://github.com/skierkowski/hello and `skierkowski/hello#new-feature-3` references the new-feature-3 branch.

# Repositories

## Commit Commet Listener
### github::repo::commit_comment
The Commit Comment Listener (`github::repo::commit_comment`) is triggered when a commit comment is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#commitcommentevent).

### Example

```ruby
listen 'github::commit_comment', repo:'skierkowski/hello' do |comment|
  info "#{comment.user.login} just commented '#{comment.body}' on #{comment.repository.full_name}"
end
```

## Create Listener
### github::repo::create
The Create Listener (`github::repo::create`) listeners for the creation of new branches, or tags. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#createevent).

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

### Example

```ruby
listen 'github::deployment', repo:'skierkowski/hello' do |deploy|
  info "The #{deploy.name}/#{deploy.ref} is getting deployed to the #{deploy.environment} environment."
end
```

## Deployment Status Listener
### github::repo::deployment_status
The Deployment Status Listener (`github::repo::deployment_status`) is triggered when the status of a deployment changes. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deploymentstatusevent).

### Example

```ruby
listen 'github::deployment_status', repo:'skierkowski/hello' do |deploy|
  info "The #{deploy.name}/#{deploy.ref} deployment status changed to #{deploy.status} for the deployment to the #{deploy.deployment.environment} environment"
end
```
## Download Listener
### github::repo::download
The Download Listener (`github::repo::download`) is triggered when a download is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#downloadevent).

### Example

```ruby
listen 'github::download', repo:'skierkowski/hello'
  success "A new download was created"
end
```

## Fork Listener
### github::repo::fork
The Fork Listener (`github::repo::fork`) is triggered when the repo is forked. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#forkevent).

### Example

```ruby
listen 'github::fork', repo:'skierkowski/hello' do |fork|
  info "#{fork.forkee.name} just forked #{fork.repository.full_name}"
end
```

## Gollum Listener
### github::repo::gollum
The Gollum Listener (`github::repo::gollum`) is triggered when a Wiki page is created or updated. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#gollumevent).

### Example

```ruby
listen 'github::gollum', repo:'skierkowski/hello' do |wiki|
  info "#{wiki.pages.count} pages have been created or updated in #{wiki.repository.full_name} by #{wiki.sender.login}"
end
```

## Comment Listener
### github::repo::issue_comment
The Issue Comment Listener (`github::repo::issue_comment`) is triggered when an issue comment is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#issuecommentevent).

### Example
```ruby
listen 'github::issue_comment', repo:'skierkowski/hello' do |comment|
  info "#{comment.sender.login} #{comment.action} a comment on issue #{comment.issue.title} with `#{comment.comment.body}`"
end
```

## Issue Listener
### github::repo::issues
The Issues Listener (`github::repo::issues`) is triggered when an issue is assigned, unassigned, labeled, unlabeled, opened, closed, or reoponed. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#issuesevent).

### Example
```ruby
listen 'github::issues', repo:'skierkowski/hello' do |issue|
  info "#{issue.sender.login} #{issue.action} an issue titled #{issue.title}"
end
```

## Member Listenr
### github::repo::member
The Member Listener (`github::repo::member`) is triggered when a user is added as a collaborator to a repository. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#memberevent).

### Example
```ruby
listen 'github::member', repo:'skierkowski/hello' do |issue|
  info "#{member.login} was #{action} to the repo #{issue.repository.full_name}"
end
```

## Page Build Listener
### github::repo::page_build
The Page Build Listener (`github::repo::page_build`) is triggered when an attempt is made to build a Github Page site, whether successful or not. This is triggered on push to a Github Pages enabled branch, either `gh-pages` for project pages, or `master` for user/organization pages. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pagebuildevent).

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

### Example
```ruby
listen 'github::public', repo: 'factor-io/factor' do |repo|
  success "Yey, we went open source!"
end
```

## Pull Request Listener
### github::repo::pull_request
The Pull Request Listener (`github::repo::pull_request`) is triggered when a pull request is assigned, unassigned, labeled, unlabeled, opened, closed, reopened, or synchronized. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pullrequestevent).

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

### Example
```ruby
listen 'github::pull_request_review_comment', repo:'skierkowski/hello' do |comment|
  info "#{comment.sender.login} #{comment.action} commented #{comment.comment.body} on #{comment.repository.full_name}"
end
```

## Push Listener
### github::repo::push
The Push Listener (`github::repo::push`) is triggered when a repository branch is pushed to. This is the most used listener as it is often used to invoke deployments when code is pushed or merged into the master branch. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pushevent).

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

### Example
```ruby
listen 'github::release', repo:'skierkowski/hello#master' do |release|
  info "Release #{release.release.tag_name} was #{release.action}"
end
```
## Status Listener
### github::repo::status
The Status Listener (`github::repo::status`) is triggered when the status of a git commit changes. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#statusevent).

### Example
```ruby
listen 'github::status', repo:'skierkowski/hello' do |status|
  info "Commit #{status.commit.message} was a #{status.state}"
end
```

## Team Add Listener
### github::repo::team_add
The Team Add listener (`github::repo::team_add`) is triggered when a user or repository is added to a team. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#teamaddevent).

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

### Example
```ruby
listen 'github::watch', repo:'skierkowski/hello' do |watch|
  info "#{watch.sender.login} #{watch.action} repo #{watch.repository.full_name}"
end
```

# Deployments

## List Deployments Action
### github::deployments::list

## Create Deployment Action
### github::deployments::create

## List Deployment Statuses Action
### github::deployments::statuses

## Create Deployment Status Action
### github::deployments::create_status

# Issues

## List Issues Action
### github::issues::list

## Get Issue Action
### github::issues::get

## Create Issue Action
### github::issues::create

## Edit Issue Action
### github::issues::edit

## Close Issue Action
### github::issues::close

# Releases

## List Releases Action
### github::releases::list

## Get Release Action
### github::releases::get

## Create Release Action
### github::releases::create

## Delete Relase Action
### github::releases::delete

# Statuses

## List Statuses Action
### github::statuses::list

## Creat Statuses Action
### github::statuses::create

