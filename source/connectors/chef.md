---
title: Chef
category: Connectors
---
This Chef Connector enables you to bootstrap a server using both Chef Server and Hosted Chef, but not Chef Solo.

# Authentication
ID | Default | Description
--- | ------- | -----------
private_key |  | The Private Key chef needs to SSH into the remote server
validation_key |  | The validation key provided by Chef Server. Typically found in `~/.chef`.
organization |  | The organization name as managed in Chef Server.
chef_server | https://api.opscode.com/organizations/{organization} | The Chef Server URL.

**Note**: Either the organization or chef_server value must be set, but not both. If the organization is provided the Chef Server URL will be generated to reference Hosted Chef, if you provided the Chef Server URL you can connect to your own Chef Server.

Below is an example credentials.yml file with the above values.

**credential.yml**

```yaml
---
chef:
  organization: factor
  validation_key: |
    -----BEGIN RSA PRIVATE KEY-----
    MIIEowIBAAKCAQEA+xYrU/kmT8UMWPbquT/FpNkkgybuIv/S19G4jgLoM8NvuWAe
    yZW7/XZADsGr7R14TrasBrHH9nxgLsSkSAfpNOlzctC2sYfnB7dlbD9V1XO8ySTe
    fJLmXASdTv4PkqpgaoJXpC1OxiJFFnxfwHLwrSHG2NgAvl4Cvd4WQ4epMwt7+KeX
    KrC4eUsqpUQmR6wFAdiP6/JivH8BhfS8AKvbklQxEApGVZsLUw4AR9eUEt6+lllh
    8gTI/H5V6o2GygXTdNrsoR6ZypHeIYd6S2Yeuzb9K4miQdHpHmrN+40BpyeFfLhQ
    fWSJ5OPHyb6chnZ9RqcsmLz9vTX/HJTNGRP9a/1g8KWx1/8g7xRH3vBZHsBGmZTq
    X0J+axPjLSywicOdHG8W+eANGu8F712V2xXrr6t38HjCu/FcG1xrFghphUOPQ6hH
    mswCbQKBgFGldEJ+pDzXGbejNuZlEn3tz3iWpXkPMUhPR+hjYvn5IJkd4K/8kEQ8
    SaGHjsVaEhasQFKyJiS1YgY/roecW9F/hC/G6az3IQjJjMwNdtcaoeL4ejqi29/B
    gHdaotcmeAK+enA02Z7n3Yk0XQpMGe28Tyl57CBlRszbrdrojxdM
    -----END RSA PRIVATE KEY-----
  private_key: |-
    -----BEGIN RSA PRIVATE KEY-----
    MIIEowIBAAKCAQEAgZfPXnu9RP1rBrUjiRN1piACKw/pjRLZ1amY68cmbNM45OjQrfbuOE2iAvvX
    ZU7euTXKPwVHGbAOqSK2VIMtVmOKmMxDzrC7bcIes/WvbKL6wm6NqAPUZGmLHUXb3bJDEfijL8fl
    nc3lp1jGyYmaPYBuRXL94VVZnUEKRSiLYTTdO+1V1abOgFCL/E3gI3AwAbbEgLPIDxYHVJ063JED
    0Oa0h9d6ofqlBQErQG+rOzGUUSSLv9yDnlnVQJB7wj/6HMv8lBZ9HsAXf5Hl4PPd+ptVateyf3cK
    VV7lIZ8EL6C7pjVZwhMhoXCouQ2tPyJhBTKVHHf9ipnvo8MM7zuq3QIDAQABAoIBACpTBmr5Rstt
    rP37dvVgaQYgplgE9pfZxf0CgYBjcNSgvq4cC5TKC5ob7ir3PNlWdvXyHO172JcLBKR/rQmM7EPm
    nSXTKtKkDVF5fIeh9m3EgenvzmrVPcXx/mRFPmKsAfJ0VkN6bXVP1557WsFpKq2XBZNIhCjaGEko
    +CvhsOCPlaab5TYi+PvlAe3AfFTu0rb+4Tu0jcpXqNA/tQKBgGV28L35cCplxtjznJzBRA+XVXvA
    L87ipmp3EPz8nOQm2BUg2Stmk/r28xaonQZrYBqeiyrRS7SwfWBP42/N7HfArDriwWahm8A3dLVl
    BGtjWhInjKCeP0ROk/tlMRHZ39pGhroiKjmrxg0jgDjy+WLmh4DRx1YuWLmoJkxeSzt5
    -----END RSA PRIVATE KEY-----
```

# chef::bootstrap

## Input parameters
ID | Default | Description
--- | ------- | -----------
host | false | The host address for the server to bootstrap, including the username (e.g. `ubuntu@10.03.221.02`)
runlist | false | The Chef runlist for bootstrapping. e.g. `role[console]`
name | false | The name of the server as it will be captured on the chef server
environment | _default | The environment you'd like to use for this server (e.g. `production`)

## Output
The output is rather uneventfull. The logs show all the details of the procedure, but the output only shows `{output:'complete'}` upon completion.
```json
{
  output: 'complete'
}
```

## Example

```ruby
listen 'web::hook', id:'provision' do |post_options|
  host = post_options.host
  name = post_options.name
  run 'chef::bootstrap', host: host, name:name, runlist:'role[console]' do |run_complete|
    success "Finished bootstrapping #{name} '#{host}'"
  end.on_fail
    error "Failed bootstrapping #{name} '#{host}'"
  end
end

# http POST https://open-connectors.factor.io/v0.4/hooks/provision host:'root@sandbox.factor.io' name:'Cloud-Server-2'
```
