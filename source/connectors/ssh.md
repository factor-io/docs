---
title: SSH
category: Connectors
connector_type: Remote Operations
logo: https://factor.io/assets/channel_logos/ssh.png
---
The SSH Connector enables you to execute commands on an SSH server or upload files via SCP.

# Authetication
The SSH commands currently support authentication by SSH Keys only, no U/P support. While the remote server has your public key to validate your authenticitity, the SSH client requires the private key. As such, you need to paste in your pirvate SSH key into the credentials.yml file. In YML you will need to put the "|" character follwing the ":" and a space in between. Thereafter you can put in the multi-line SSH private key indented one tab (two spaces).

**credentials.yml**

```yaml
ssh:
  private_key: |
    -----BEGIN RSA PRIVATE KEY-----
    MIIEowIBAAKCAQEAgZfPXnu9RP1rBrUjiRN1piACKw/pjRLZ1amY68cmbNM45OjQrfbuOE2iAvvX
    ZU7euTXKPwVHGbAOqSK2VIMtVmOKmMxDzrC7bcIes/WvbKL6wm6NqAPUZGmLHUXb3bJDEfijL8fl
    nc3lp1jGyYmaPYBuRXL94VVZnUEKRSiLYTTdO+1V1abOgFCL/E3gI3AwAbbEgLPIDxYHVJ063JED
    0Oa0h9d6ofqlBQErQG+rOzGUUSSLv9yDnlnVQJB7wj/6HMv8lBZ9HsAXf5Hl4PPd+ptVateyf3cK
    nSXTKtKkDVF5fIeh9m3EgenvzmrVPcXx/mRFPmKsAfJ0VkN6bXVP1557WsFpKq2XBZNIhCjaGEko
    +CvhsOCPlaab5TYi+PvlAe3AfFTu0rb+4Tu0jcpXqNA/tQKBgGV28L35cCplxtjznJzBRA+XVXvA
    L87ipmp3EPz8nOQm2BUg2Stmk/r28xaonQZrYBqeiyrRS7SwfWBP42/N7HfArDriwWahm8A3dLVl
    BGtjWhInjKCeP0ROk/tlMRHZ39pGhroiKjmrxg0jgDjy+WLmh4DRx1YuWLmoJkxeSzt5
```

# ssh::execute
The Remote Execute (`ssh::execute`) action allows you to execute commands on a remote server.

## Parameters
- **host** (required): The server host including the username, host, and port (optionally). e.g. "ubuntu@sandbox.factor.io" or "root@foo.com:256"
- **commands** (required): This is an array of strings with the commands to be executed.

## Results
After you run the command you will get the results in the following variables:

- **all**: all stdout/stderr from all commands in a single string
- **lines[n]**: the n-th line of stdout/sterror from all commands
- **command[n].all**: all stdout/sterr from the n-th command in a single string
- **command[n].lines[m]**: the m-th line from the n-th command

## Example

```ruby
listen 'timer::every', minutes:10 do
  run 'ssh::execute', host:'ubuntu@sandbox.factor.io', commands:['pwd'] do |ssh_results|
    info "Path: #{ssh_results.commands[0].lines[0]}"
  end
end
```


# ssh::upload
The SCP Upload action (`ssh::upload`) is used to upload a file to a server via SCP (secure copy over SSH).

## Parameters
- **host** (required): The server host including the username, host, and port (optionally). e.g. "ubuntu@sandbox.factor.io" or "root@foo.com:256"
- **content** (required): The reference to the contents that is to be deployed. The contents is retreived from anther listener (e.g. github::push) or action (e.g. github::download)
- **path** (required): The absolute remote destination path for the file. This is to include the filename. Make sure the user has write permissions.

## Example

```ruby
listen 'github::push', repo:'skierkowski/hello#master' do |github_info|
  run 'ssh::upload', host:'ubuntu@sandbox.factor.io', path:'/home/ubuntu/web.zip', content:github_info.content do |deploy_info|
    info 'Deployment is complete'
  end
end
```

