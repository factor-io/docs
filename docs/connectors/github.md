# Authentication
Github requires an API Key (api_key) to be defined in your credentials.yml file. To get the API key go to your [Application Settings page in Github](https://github.com/settings/applications) and use the "Generate new token" button in the Personal access tokens section to generate a new token. Copy the new key and paste it into your credentials.yml file.

**credentials.yml**

    github:
      api_key: 4686b1488574a768a8a8d

# Parameters
All of the listeners below require the same set of parameters

- **repo**: The address of the repo in the format (user/repo#branch). The use rand repo in the format are required while the branch is optional. If the branch is not specified it will default to `master`. For example `skierkowski/hello` would refer to https://github.com/skierkowski/hello and `skierkowski/hello#new-feature-3` references the new-feature-3 branch.

# github::commit_comment
The Commit Comment Listener (`github::commit_comment`) is triggered when a commit comment is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#commitcommentevent).

## Example
    listen 'github::commit_comment', repo:'skierkowski/hello' do |comment|
      info "#{comment.user.login} just commented '#{comment.body}' on #{comment.repository.full_name}"
    end

# github::create
The Create Listener (`github::create`) listeners for the creation of new branches, or tags. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#createevent).

## Example
    listen 'github::create', repo:'skierkowski/hello' do |create_event|
      if create_event.ref_type == 'tag'
        info "#{create_event.sender.login} just created a new tag called #{create_event.ref}"
      elsif create_event.ref_type == 'branch'
        info "#{create_event.sender.login} just created a new branch called #{create_event.ref}"
      end
    end

# github::delete
The Delete Listener (`github::delete`) listeners for the deleting of new branches, or tags. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deleteevent).

## Example
    listen 'github::delete', repo:'skierkowski/hello' do |delete_event|
      if delete_event.ref_type == 'tag'
        info "#{delete_event.sender.login} just deleted a new tag called #{delete_event.ref}"
      elsif delete_event.ref_type == 'branch'
        info "#{delete_event.sender.login} just deleted a new branch called #{delete_event.ref}"
      end
    end

# github::deployment
The Deployment Listener (`github::deployment`) is triggered on new deployments. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deployevent).

## Example
    listen 'github::deployment', repo:'skierkowski/hello' do |deploy|
      info "The #{deploy.name}/#{deploy.ref} is getting deployed to the #{deploy.environment} environment."
    end

# github::deployment_status
The Deployment Status Listener (`github::deployment_status`) is triggered when the status of a deployment changes. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#deploymentstatusevent).

## Example
    listen 'github::deployment_status', repo:'skierkowski/hello' do |deploy|
      info "The #{deploy.name}/#{deploy.ref} deployment status changed to #{deploy.status} for the deployment to the #{deploy.deployment.environment} environment"
    end

# github::download
The Download Listener (`github::download`) is triggered when a download is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#downloadevent).

## Example
    listen 'github::download', repo:'skierkowski/hello'
      success "A new download was created"
    end

# github::fork
The Fork Listener (`github::fork`) is triggered when the repo is forked. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#forkevent).

## Example
    listen 'github::fork', repo:'skierkowski/hello' do |fork|
      info "#{fork.forkee.name} just forked #{fork.repository.full_name}"
    end

# github::gollum
The Gollum Listener (`github::gollum`) is triggered when a Wiki page is created or updated. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#gollumevent).

## Example
    listen 'github::gollum', repo:'skierkowski/hello' do |wiki|
      info "#{wiki.pages.count} pages have been created or updated in #{wiki.repository.full_name} by #{wiki.sender.login}"
    end

# github::issue_comment
The Issue Comment Listener (`github::issue_comment`) is triggered when an issue comment is created. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#issuecommentevent).

## Example
    listen 'github::issue_comment', repo:'skierkowski/hello' do |comment|
      info "#{comment.sender.login} #{comment.action} a comment on issue #{comment.issue.title} with `#{comment.comment.body}`"
    end

# github::issues
The Issues Listener (`github::issues`) is triggered when an issue is assigned, unassiagned, labeled, unlabeled, opened, closed, or reoponed. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#issuesevent).

## Example
    listen 'github::issues', repo:'skierkowski/hello' do |issue|
      info "#{issue.sender.login} #{issue.action} an issue titled #{issue.title}"
    end

# github::member
The Member Listener (`github::member`) is triggered when a user is added as a collaborator to a repository. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#memberevent).

## Example
    listen 'github::member', repo:'skierkowski/hello' do |issue|
      info "#{member.login} was #{action} to the repo #{issue.repository.full_name}"
    end


# github::page_build
The Page Build Listener (`github::page_build`) is triggered when an attept is made to build a Github Page site, whether successful or not. This is triggered on push to a Github Pages enabled branch, either `gh-pages` for project pages, or `master` for user/organization pages. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pagebuildevent).

## Example
    listen 'github::page_build', repo:'skierkowski/hello' do |page|
      if page.build.status == 'built'
        success "#{page.build.pusher.login} successfully built #{page.repository.full_name}"
      else
        fail "Failed to build #{page.repository.full_name}"
      end  
    end


# github::public
The Public Listener (`github::public`) is triggered when a repo is open sourced. We need more of these. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#publicevent).

## Example
    listen 'github::public', repo: 'factor-io/factor' do |repo|
      success "Yey, we went open source!"
    end

# github::pull_request
The Pull Request Listener (`github::pull_request`) is triggered when a pull request is assigned, unassigned, labeled, unlabeled, opened, closed, reopened, or synchronized. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pullrequestevent).

## Example
    listen 'github::pull_request', repo:'skierkowski/hello' do |request|
      msg = "Pull request #{request.number} was #{request.action}: #{reqest.pull_request.title}"
      run 'hipchat::send', room:'Engineering', message: msg
    end


# github::pull_request_review_comment
The Pull Request Review Comment Listener (`github::pull_request_review_comment`) is triggered when a comment is created on a portion of the unified diff of a pull request. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pullrequestreviewcommentevent).

## Example
    listen 'github::pull_request_review_comment', repo:'skierkowski/hello' do |comment|
      info "#{comment.sender.login} #{comment.action} commented #{comment.comment.body} on #{comment.repository.full_name}"
    end

# github::push
The Push Listener (`github::push`) is triggered when a repository branch is pushed to. This is the most used listener as it is often used to invoke deployments when code is pushed or merged into the master branch. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#pushevent).

## Example
    listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
      run 'heroku::deploy', app:'hello-frank', content:github_info.content do |deploy_info|
        info 'Deployment is complete'
      end
    end

# github::release
The Release Listener (`github::release`) is triggered when a release is published. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#releaseevent).

## Example
    listen 'github::release', repo:'skierkowski/hello#master' do |release|
      info "Release #{release.release.tag_name} was #{release.action}"
    end

# github::status
The Status Listener (`github::status`) is triggered when the status of a git commit changes. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#statusevent).

## Example
    listen 'github::status', repo:'skierkowski/hello' do |status|
      info "Commit #{status.commit.message} was a #{status.state}"
    end

# github::team_add
The Team Add listener (`github::team_add`) is triggered when a user or repository is added to a team. The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#teamaddevent).

## Example
    listen 'github::team_add', repo:'skierkowski/hello' do |add_info|
      if add_info.team
        info "Team #{add_info.team.name} was added to #{add_info.repository.full_name}"
      elsif add_info.user
      info "Team #{add_info.user.login} was added to #{add_info.repository.full_name}"
      end
    end

# github::watch
The Watch Listener (`github::watch`) is triggered when a user Stars a repository (not watches). The details of the returned output can be found [here](https://developer.github.com/v3/activity/events/types/#watchevent).

## Example
    listen 'github::watch', repo:'skierkowski/hello' do |watch|
      info "#{watch.sender.login} #{watch.action} repo #{watch.repository.full_name}"
    end

