# Travis

## Authentication
Authentication can be achieve with either an access token or a github token. 

- **access_token**: Your Travis access token which you can obtain from the command line via `travis login`
- **github_token**: Instead of a Travis token you can also use a Github token which you can obtain from Account Settings > Applications > Personal access token

**credentials.yml**
    travis:
      access_token: wwhtBp58xbM88sBdwHnRfg

## Rebuild
The Rebuild Action (`travis::rebuild`) can be used to re-run a recent build.

### Parameters
- **repo**: the repo slug (e.g. rails/rails) for the build you want to rebuild
- **pro**: (optional, default:false) whether you are using Travis Pro version (private repos)
- **build_number**: (optional) the build number you want to re-build. By default will use the last build.

### Output
- **repository_id**: The Github repository ID
- **commit_id**: Last commit ID for the build
- **number**: The build number being rebuilt
- **pull_request**: Message from the pull request for the build
- **pull_request_number**: Github Pull Request number
- **pull_request_title**: Github Pull Request title
- **state**: Build state
- **started_at**: Start time of build (may be nill)
- **finished_at**: End time of build (may be nill)
- **duration**: Duration fo build (may be nill)
- **job_ids**: Job IDs for the build

### Example
    listen 'timer::every', minutes:60 do |timer_info|
      run 'travis::rebuild', repo:'skierkowski/hello' do |build|
        info "Restarting build # #{build.number} for repo #{build.id}"
      end
    end

