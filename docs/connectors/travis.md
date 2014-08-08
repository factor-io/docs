# Travis

## Credentials
- **access_token**: Your Travis access token which you can obtain from the command line via `travis login`
or...
- **github_token**: Instead of a Travis token you can also use a Github token which you can obtain from Account Settings > Applications > Personal access token

## Rebuild

- **repo**: the repo slug (e.g. rails/rails) for the build you want to rebuild
- **pro**: (optional, default:false) whether you are using Travis Pro version (private repos)
- **build_number**: (optional) the build number you want to re-build. By default will use the last build.
