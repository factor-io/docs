---
title: Chef
category: Connectors
connector_type: Configuration Management
logo: https://raw.githubusercontent.com/factor-io/connector-chef/master/logo.png
---
This Chef Connector enables you to bootstrap a server using both Chef Server and Hosted Chef, but not Chef Solo.

# Authentication
ID | Default | Description
--- | ------- | -----------
client_key | (required) | Your Chef Client privagte key
client_name | (required) | The name of the client associated with the private client key.
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
  client_key: |-
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
  client_name: factor-test
```

# Knife

## Additional Authentication
All other supported operations use the Chef Server API to perform CRUD operations. However, the `chef::knife` commands require that you SSH into the node which you will be managing. As such, you need to provide additional credentials in your `credentials.yml` file.

ID | Default | Description
--- | ------- | -----------
private_key | (required) | The SSH Private Key to connect to the server (as if you were using `ssh` from the command-line).
validation_key | (required)  | The validation key provided by Chef Server. Typically found in `~/.chef`. The `chef::knife::bootstrap` operations creates a new client (node) with a Private Key, as such, you do not need to provide a `client_key` as that will be generated; however, you do need to providate the `validation_key` as provided by Chef Server.

## Knife Bootstrap

### chef::bootstrap

### Input parameters
ID | Default | Description
--- | ------- | -----------
host | false | The host address for the server to bootstrap, including the username (e.g. `ubuntu@10.03.221.02`)
runlist | false | The Chef runlist for bootstrapping. e.g. `role[console]`
name | false | The name of the server as it will be captured on the chef server
environment | _default | The environment you'd like to use for this server (e.g. `production`)

### Output
The output is rather uneventfull. The logs show all the details of the procedure, but the output only shows `{output:'complete'}` upon completion.

### Example

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

# Clients

## Get all clients
### chef::clients::all
### Parameters
No additional parameters

### Output
ID | Description
--- | -----------
[] | An array of clients represented as Hashs
[n]['name'] | Client Name
[n]['admin'] | Boolean (true/false) whether this client is an Admin
[n]['private_key'] | True / False whether the private key is set
[n]['public_key'] | The client's public key, or false if one isn't set
[n]['validator'] | 

## Get a client
### chef::clients::get
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Client ID (name)

### Output
ID | Description
--- | -----------
['name'] | Client Name
['admin'] | Boolean (true/false) whether this client is an Admin
['private_key'] | True / False whether the private key is set
['public_key'] | The client's public key, or false if one isn't set
['validator'] | 

## Create a client
### chef::clients::create
### Parameters
ID | Default | Description
--- | ------- | -----------
name | (required) | The name of the new client
admin | (optional) | Boolean whether this client is admin
public_key | (optional) | Provide a public key. One will be generated if not provided.
private_key | (optional) | Client private key, one will be generated if not provivded
validator | (optional) | 


### Output
This will return a Hash of the vlaues that were recorded when client was created

ID | Description
--- | -----------
['name'] | Client Name
['admin'] | Boolean (true/false) whether this client is an Admin
['private_key'] | True / False whether the private key is set
['public_key'] | The client's public key, or false if one isn't set
['validator'] | 

## Delete a client
### chef::clients::delete
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | Client ID (name) to delete

### Output
Nothing.

# Data Bags & Items

## Get all Data Bags
### chef::databags::all
### Parameters
None. It will get all data bags.

### Output
ID | Description
--- | -----------
[] | An array of Data Bags
[n]['name'] | The Databag Name

## Get a Data Bag
### chef::databags::get
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Data Bag ID (name)

### Output
ID | Description
--- | -----------
['name'] | The Databag Name

## Create a Data Bag
### chef::databags::create
### Parameters
ID | Default | Description
--- | ------- | -----------
name | (required) | The Data Bag Name

### Output
ID | Description
--- | -----------
['name'] | The name of the Data Bag

## Update a Data Bag
### chef::databags::update
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Data Bag ID (the old name)
name (required) | The new Data Bag Name

### Output
ID | Description
--- | -----------
['name'] | The new name of the Data Bag

## Delete a Data Bag
### chef::databags::delete
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Data Bag ID (name)

### Output
Nothing.

## List all Data Bag Items
### chef::databags::items
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Data Bag ID (name)

### Output
ID | Description
--- | -----------
[] | An array of all the databags
[n].keys | The keys in the Hash are the Databag Item IDs
[n].values | The data of the data bag items returned as a Hash

## Get a Data Bag Item
### chef::databags::get_item
### Parameters
ID | Default | Description
--- | ------- | -----------
databag | (required) | The Data Bag ID (name)
id | (required) | The Item ID


### Output
ID | Description
--- | -----------
{} | A hash of the data bag item data

## Update a Data Bag Item
### chef::databags::update_item
This will update the contents of the Data Bag Item data. It uses a deep merge to update the data, so existing key/values in the tree will remain as-is.

### Parameters
ID | Default | Description
--- | ------- | -----------
databag | (required) | The Data Bag ID (name)
id | (required) | The Item ID
data | (required) | A Hash of the Data Bag Item data

### Output
ID | Description
--- | -----------
{} | The new data bag item contents

## Create a Data Bag Item
### chef::databags::create_item
### Parameters
ID | Default | Description
--- | ------- | -----------
databag | (required) | The Data Bag ID (name)
id | (required) | The Item ID
data | (required) | A Hash of the Data Bag Item data

### Output
ID | Description
--- | -----------
{} | The new data bag item contents

## Delete a Data Bag Item
### chef::databags::delete_item
### Parameters
ID | Default | Description
--- | ------- | -----------
databag | (required) | The Data Bag ID (name)
id | (required) | The Item ID

### Output
Nothing

# Environments

## Get all Environments
### chef::environments::all
### Parameters
No input required.

### Output
ID | Description
--- | -----------
[] | An array of environments
[n]['name'] | Name of the environment
[n]['description'] | Description of the environment
[n]['default_attributes'] | A Hash of the default attributes for the environment
[n]['override_attributes'] | A Hash of the override attributes for the environment
[n]['cookbook_versions'] | A Hash (usually) for the environemnt

## Get an Environment
### chef::environments::get
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Environment ID (name)

### Output
ID | Description
--- | -----------
['name'] | Name of the environment
['description'] | Description of the environment
['default_attributes'] | A Hash of the default attributes for the environment
['override_attributes'] | A Hash of the override attributes for the environment
['cookbook_versions'] | A Hash (usually) for the environemnt

## Create an Environment
### chef::environments::create
### Parameters
ID | Default | Description
--- | ------- | -----------
name | (required) | Name of the environment
description | (optional) | Description of the environment
default_attributes'] | {} | A Hash of the default attributes for the environment
override_attributes'] | {} | A Hash of the override attributes for the environment
cookbook_versions'] | {} | A Hash (usually) for the environemnt

### Output
ID | Description
--- | -----------
['name'] | Name of the environment
['description'] | Description of the environment
['default_attributes'] | A Hash of the default attributes for the environment
['override_attributes'] | A Hash of the override attributes for the environment
['cookbook_versions'] | A Hash (usually) for the environemnt

## Update an Environment
### chef::environments::update
This updates the environment settings. In particular, it uses a deep merge for the `default_attributes`, `override_attributes` and `cookbook_versions`.
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | Name of the environment
description | (optional) | Description of the environment
default_attributes'] | {} | A Hash of the default attributes for the environment
override_attributes'] | {} | A Hash of the override attributes for the environment
cookbook_versions'] | {} | A Hash (usually) for the environemnt

### Output
ID | Description
--- | -----------
['name'] | Name of the environment
['description'] | Description of the environment
['default_attributes'] | A Hash of the default attributes for the environment
['override_attributes'] | A Hash of the override attributes for the environment
['cookbook_versions'] | A Hash (usually) for the environemnt

## Delete an Environment
### chef::environments::delete
### Parameters
ID | Default | Description
--- | ------- | -----------
id | (required) | The Environemnt ID (name)

### Output
Nothing.