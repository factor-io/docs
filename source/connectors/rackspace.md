---
title: Rackspace
category: Connectors
---
# Authentication
Rackspace requires a username, API Key, and for certain actions an SSH Key. To obtain your Username and API Key go to the **[Cloud Control Panel](https://mycloud.rackspace.com/)** > **Account Settings**. There you will find "Username" and "API Key" information.

**credentials.yml**
```yml
rackspace:
  api_key: d3aea086db586db5e981c28bfe852d3ae
  username: skierkowski
```

For operations like bootstrapping a new server, you will also need to provide public private key pair.

```yml
---
rackspace:
  api_key: d3aea086db586db5e981c28bfe852d3ae
  username: skierkowski
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
  public_key: |
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDP9JdVFd56cahzkUkwngfZeNQoxxr3db3Qg0z4CB6Bn5FggJSiBgFGnfTZWXopthsuv4xYu80RemG0A8pNmkbTWqRW1XVUzXsnMbFE+ag8ZUbl4+bLEyftpYGfHoIYf3cnVvmW5HEck8HG+MoQEwsoH31Z7SbJKBPoj3nXEe6KY5XJwxyQAM10ArHK+kanXa1v5je0pRxkNZUKAOpm20DGyZjogFK4/SpR5MWV8Q375L5ojVDX4EAon9mFKgMHs6MY1t3UINB2S2GrX09Y16OC6mqYNyQdOc4AnIGKfD07+D4wC4GQi6GpYZdCCrnryIhA9PM8iSeP6AXbeRFcScNt

```

### Setting up the Connectors
Currently Rackspace supports operations on Compute, Flavors, and Images, with more coming soon. Each of these resources are hosted on individual endpoint addresses. As such, we have a recommended definition of your connectors.yml file.

### connectors.yml
```yml
---
bitballoon: wss://open-connectors.factor.io/v0.4/bitballoon
bitbucket: wss://open-connectors.factor.io/v0.4/bitbucket
github: wss://open-connectors.factor.io/v0.4/github
heroku: wss://open-connectors.factor.io/v0.4/heroku
hipchat: wss://open-connectors.factor.io/v0.4/hipchat
middleman: wss://open-connectors.factor.io/v0.4/middlemanb
rackspace:
  compute: wss://open-connectors.factor.io/v0.4/rackspace_compute
  flavors: wss://open-connectors.factor.io/v0.4/rackspace_images
  images: wss://open-connectors.factor.io/v0.4/rackspace_flavors
ssh: wss://open-connectors.factor.io/v0.4/ssh
timer: wss://open-connectors.factor.io/v0.4/timer
web: wss://open-connectors.factor.io/v0.4/web
```

# rackspace::compute::list

## Input
- **region** (optional, default:'ord'): A string containing the three letter region

## Output
Output is an Array of Hashes. The Factor Server automatically converts this to a nested OStruct, so you can address variables like `servers.first.addresses.public.last.addr`.
```ruby
[
  {
    :state => "ACTIVE",
    :updated => "2014-09-30T17:15:16Z",
    :host_id => "ffec79d5145436b031973879740ce2736c21d3da913deaa0bccf6561",
    :addresses => {
      :public => [
        {
          :version => 6,
          :addr => "2001:4800:7813:516:d02:b50b:5bdb:71ab"
        },
        {
          :version => 4,
          :addr => "192.237.202.61"
        }
      ],
      :private => [
        {
          :version => 4,
          :addr => "10.183.7.124"
        }
      ]
    },
    :links => [
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
        :rel => "self"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
        :rel => "bookmark"
      }
    ],
    :key_name => "Factor",
    :image_id => "34437b38-6df1-4efa-bded-f637d8864b83",
    :state_ext => nil,
    "OS-EXT-STS:vm_state" => "active",
    :flavor_id => "3",
    :id => "1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
    :user_id => "10045058",
    :name => "console-01",
    :created => "2014-09-30T17:13:26Z",
    :tenant_id => "843739",
    :disk_config => "AUTO",
    :ipv4_address => "192.237.202.61",
    :ipv6_address => "2001:4800:7813:516:d02:b50b:5bdb:71ab",
    :progress => 100,
    "OS-EXT-STS:power_state" => 1,
    :config_drive => ""
  }
]
```

## Example
```ruby
listen 'github::push', repo:'factor-io/console' do |github_info|
  run 'rackspace::compute::list', region:'dfw' do |servers|
    servers.each do |server|
      if server.name =~ /console/
        run 'rackspace::compute::ssh', id: server.id, commands:['./deploy'] do |ssh_info|
          if ssh_info.commands[0].lines.last == 'success'
            success "Successfully deployed app"
          else
            fail "Failed to deploy app"
          end
        end
      end
    end
  end
end
```
# rackspace::compute::get

## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **id** (required): This is the server ID for which you want to get details. You can obtain a list of servers from `rackspace::compute::list`.

## Output
Output is a Hash of details for that server. The Factor Server automatically converts this to a nested OStruct, so you can address variables like `server.addresses.public.last.addr`.
```ruby
{
  :state => "ACTIVE",
  :updated => "2014-09-30T17:15:16Z",
  :host_id => "ffec79d5145436b031973879740ce2736c21d3da913deaa0bccf6561",
  :addresses => {
    :public => [
      {
        :version => 6,
        :addr => "2001:4800:7813:516:d02:b50b:5bdb:71ab"
      },
      {
        :version => 4,
        :addr => "192.237.202.61"
      }
    ],
    :private => [
      {
        :version => 4,
        :addr => "10.183.7.124"
      }
    ]
  },
  :links => [
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "self"
    },
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "bookmark"
    }
  ],
  :key_name => "Factor",
  :image_id => "34437b38-6df1-4efa-bded-f637d8864b83",
  :state_ext => nil,
  "OS-EXT-STS:vm_state" => "active",
  :flavor_id => "3",
  :id => "1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
  :user_id => "10045058",
  :name => "console-01",
  :created => "2014-09-30T17:13:26Z",
  :tenant_id => "843739",
  :disk_config => "AUTO",
  :ipv4_address => "192.237.202.61",
  :ipv6_address => "2001:4800:7813:516:d02:b50b:5bdb:71ab",
  :progress => 100,
  "OS-EXT-STS:power_state" => 1,
  :config_drive => ""
}
```
# rackspace::compute::create

## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **flavor_id** (required): The Flavor ID you'd like to use for the server. You can get the list of [supported flavors from Rackspace](http://docs.rackspace.com/servers/api/v2/cs-releasenotes/content/supported_flavors.html), or use the `rackspace::flavors::list` action to get a list for a particular region.
- **image_id** (required): The Image ID you'd like to use for the server. You can get the list of supported images for a region by using the `rackspace::images::list` action. 
- **name** (required): Optionally give the server a name. If one is not provided Rackspace will assign one.
- **wait** (optional, default: true): By default this will start the creation process and wait until it is completed (or fails). This can take about 10 minutes to complete, but will be ready to use once complete. By setting 'wait' to false, the creation will run asynchronously and the execution will continue without waiting for the results.

## Output
```ruby
{
  :state => "ACTIVE",
  :updated => "2014-09-30T17:15:16Z",
  :host_id => "ffec79d5145436b031973879740ce2736c21d3da913deaa0bccf6561",
  :addresses => {
    :public => [
      {
        :version => 6,
        :addr => "2001:4800:7813:516:d02:b50b:5bdb:71ab"
      },
      {
        :version => 4,
        :addr => "192.237.202.61"
      }
    ],
    :private => [
      {
        :version => 4,
        :addr => "10.183.7.124"
      }
    ]
  },
  :links => [
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "self"
    },
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "bookmark"
    }
  ],
  :key_name => "Factor",
  :image_id => "34437b38-6df1-4efa-bded-f637d8864b83",
  :state_ext => nil,
  "OS-EXT-STS:vm_state" => "active",
  :flavor_id => "3",
  :id => "1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
  :user_id => "10045058",
  :name => "console-01",
  :created => "2014-09-30T17:13:26Z",
  :tenant_id => "843739",
  :disk_config => "AUTO",
  :ipv4_address => "192.237.202.61",
  :ipv6_address => "2001:4800:7813:516:d02:b50b:5bdb:71ab",
  :progress => 100,
  "OS-EXT-STS:power_state" => 1,
  :config_drive => ""
}
```
## Example

```ruby
listen 'web::hook' do |post_info|
  image_id = post_info.image || '34437b38-6df1-4efa-bded-f637d8864b83'
  flavor_id = post_info.flavor || 2

  run 'rackspace::compute::create', region:'dfw', flavor_id:flavor_id, image_id: image_id, name:'foo' do |create_info|
    info create_info
  end
end

```

# rackspace::compute::bootstrap

## credentials.yml
This is the only action which requires the additional SSH public/private key to be specified in the `credentials.yml` file.

## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **flavor_id** (required): The Flavor ID you'd like to use for the server. You can get the list of [supported flavors from Rackspace](http://docs.rackspace.com/servers/api/v2/cs-releasenotes/content/supported_flavors.html), or use the `rackspace::flavors::list` action to get a list for a particular region.
- **image_id** (required): The Image ID you'd like to use for the server. You can get the list of supported images for a region by using the `rackspace::images::list` action. 
- **name** (required): Optionally give the server a name. If one is not provided Rackspace will assign one.
- **wait** (optional, default: true): By default this will start the creation process and wait until it is completed (or fails). This can take about 10 minutes to complete, but will be ready to use once complete. By setting 'wait' to false, the creation will run asynchronously and the execution will continue without waiting for the results.

## Output
```ruby
{
  :state => "ACTIVE",
  :updated => "2014-09-30T17:15:16Z",
  :host_id => "ffec79d5145436b031973879740ce2736c21d3da913deaa0bccf6561",
  :addresses => {
    :public => [
      {
        :version => 6,
        :addr => "2001:4800:7813:516:d02:b50b:5bdb:71ab"
      },
      {
        :version => 4,
        :addr => "192.237.202.61"
      }
    ],
    :private => [
      {
        :version => 4,
        :addr => "10.183.7.124"
      }
    ]
  },
  :links => [
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "self"
    },
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "bookmark"
    }
  ],
  :key_name => "Factor",
  :image_id => "34437b38-6df1-4efa-bded-f637d8864b83",
  :state_ext => nil,
  "OS-EXT-STS:vm_state" => "active",
  :flavor_id => "3",
  :id => "1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
  :user_id => "10045058",
  :name => "console-01",
  :created => "2014-09-30T17:13:26Z",
  :tenant_id => "843739",
  :disk_config => "AUTO",
  :ipv4_address => "192.237.202.61",
  :ipv6_address => "2001:4800:7813:516:d02:b50b:5bdb:71ab",
  :progress => 100,
  "OS-EXT-STS:power_state" => 1,
  :config_drive => ""
}
```
## Example

```ruby
listen 'web::hook' do |post_info|
  image_id = post_info.image || '34437b38-6df1-4efa-bded-f637d8864b83'
  flavor_id = post_info.flavor || 2

  run 'rackspace::compute::bootstrap', region:'dfw', flavor_id:flavor_id, image_id: image_id, name:'foo' do |create_info|
    info create_info
  end
end

```

# rackspace::compute::ssh
Use this to run SSH commands on server. You can pass a list of commands and all will be executed in the same SSH session in the order provided, and the respective results will be returned. This requires that an SSH Key was specified in the creation process, otherwise Rackspace can't identify the required SSH key.

## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **commands** (required): An array of string containing the commands to run
- **id** (required): The Server ID of server to execute commands

## Output
```ruby
[
  {
    :status => 0,
    :command => "ls -al",
    :stderr => "",
    :stdout => "total 32\r\ndrwx------  6 root root 4096 2014-10-02 17:37 .\r\ndrwxr-xr-x 22 root root 4096 2014-09-26 12:42 ..\r\ndrwx------  2 root root 4096 2014-09-26 12:44 .aptitude\r\n-rw-r--r--  1 root root 3106 2010-04-23 09:45 .bashrc\r\ndrwx------  2 root root 4096 2014-10-02 17:37 .cache\r\ndrwxr-xr-x  2 root root 4096 2014-09-26 12:42 .debtags\r\n-rw-r--r--  1 root root  140 2010-04-23 09:45 .profile\r\ndrwxr-xr-x  2 root root 4096 2014-09-30 17:15 .ssh\r\n"
  },
  {
    :status => 0,
    :command => "pwd",
    :stderr => "",
    :stdout => "/root\r\n"
  }
]
```

## Example
```ruby
listen 'github::push', repo:'factor-io/console' do |github_info|
  run 'rackspace::compute::list', region:'dfw' do |servers|
    servers.each do |server|
      if server.name =~ /console/
        run 'rackspace::compute::ssh', id: server.id, commands:['./deploy'] do |ssh_info|
          if ssh_info.commands[0].lines.last == 'success'
            success "Successfully deployed app"
          else
            fail "Failed to deploy app"
          end
        end
      end
    end
  end
end
```

# rackspace::compute::delete
This will delete a server in a region. This executes asynchronously, so it will return before the destruction process is complete.

## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **id** (required): This is the server ID for which you want to delete. You can obtain a list of servers from `rackspace::compute::list`.

## Output
Output is a Hash of details for that server. 
```ruby
{
  :state => "ACTIVE",
  :updated => "2014-09-30T17:15:16Z",
  :host_id => "ffec79d5145436b031973879740ce2736c21d3da913deaa0bccf6561",
  :addresses => {
    :public => [
      {
        :version => 6,
        :addr => "2001:4800:7813:516:d02:b50b:5bdb:71ab"
      },
      {
        :version => 4,
        :addr => "192.237.202.61"
      }
    ],
    :private => [
      {
        :version => 4,
        :addr => "10.183.7.124"
      }
    ]
  },
  :links => [
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "self"
    },
    {
      :href => "https://dfw.servers.api.rackspacecloud.com/843739/servers/1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
      :rel => "bookmark"
    }
  ],
  :key_name => "Factor",
  :image_id => "34437b38-6df1-4efa-bded-f637d8864b83",
  :state_ext => nil,
  "OS-EXT-STS:vm_state" => "active",
  :flavor_id => "3",
  :id => "1d8173b8-6efc-4f40-a134-d5cff5c1fe4a",
  :user_id => "10045058",
  :name => "console-01",
  :created => "2014-09-30T17:13:26Z",
  :tenant_id => "843739",
  :disk_config => "AUTO",
  :ipv4_address => "192.237.202.61",
  :ipv6_address => "2001:4800:7813:516:d02:b50b:5bdb:71ab",
  :progress => 100,
  "OS-EXT-STS:power_state" => 1,
  :config_drive => ""
}
```

# rackspace::compute::reboot
## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **id** (required): This is the server ID for which you want to delete. You can obtain a list of servers from `rackspace::compute::list`.

# rackspace::compute::change_password

## Input
- **region** (optional, default:'ord'): A string containing the three letter region
- **id** (required): This is the server ID for which you want to delete. You can obtain a list of servers from `rackspace::compute::list`.
- **new_password** (required): New password to use

# rackspace::flavors::list
## Input
- **region** (optional, default:'ord'): A string containing the three letter region

## Output
```ruby
[
  {
    "OS-FLV-WITH-EXT-SPECS:extra_specs" =>
    {
      "policy_class" => "standard_flavor",
      "class" => "standard1",
      "disk_io_index" => "2",
      "number_of_data_disks" => "0"
    },
    :name => "512MB Standard Instance",
    :links =>
    [
      {
        "href" => "https://dfw.servers.api.rackspacecloud.com/v2/843739/flavors/2",
        "rel" => "self"
      },
      {
        "href" => "https://dfw.servers.api.rackspacecloud.com/843739/flavors/2",
        "rel" => "bookmark"
      }
    ],
    :ram => 512,
    :vcpus => 1,
    :swap => 512,
    :rxtx_factor => 80.0,
    "OS-FLV-EXT-DATA:ephemeral" => 0,
    :disk => 20,
    :id => "2"
  },
  # truncated ...
  {
    :"OS-FLV-WITH-EXT-SPECS:extra_specs" =>
    {
      "resize_policy_class" => "performance_flavor",
      "policy_class" => "performance_flavor",
      "class" => "performance2",
      "disk_io_index" => "70",
      "number_of_data_disks" => "3"
    },
    :name => "90 GB Performance",
    :links =>
    [
      {
        "href" => "https://dfw.servers.api.rackspacecloud.com/v2/843739/flavors/performance2-90",
        "rel" => "self"
      },
      {
        "href" => "https://dfw.servers.api.rackspacecloud.com/843739/flavors/performance2-90",
        "rel" => "bookmark"
      }
    ],
    :ram => 92160,
    :vcpus => 24,
    :swap => "",
    :rxtx_factor => 7500.0,
    :"OS-FLV-EXT-DATA:ephemeral" => 900,
    :disk => 40,
    :id => "performance2-90"
  }
]
```

### Example
```ruby
listen 'hipchat::message', room:'Factor', filter:'rackspace flavors ([a-zA-Z0-1])' do |hipchat|
  criteria = hipchat.matches[0]
  run 'rackspace::flavors:list', region:'dfw' do |flavors|
    flavor_list = flavors.map{ |i| "#{i.name} (#{i.id})" }.join(', ')
    run 'hipchat::send', room:'Factor', message: "Available flavors: #{flavor_list}"
  end
end
```

# rackspace::images::list

## Input
- **region** (optional, default:'ord'): A string containing the three letter region

## Output
```ruby
[
  {
    :state                  => "ACTIVE",
    :updated                => "2014-10-16T22:50:44Z",
    :links                  => [
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/images/fe486888-6890-47ac-a02d-b740868f143b",
        :rel  => "self"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/images/fe486888-6890-47ac-a02d-b740868f143b",
        :rel  => "bookmark"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/images/fe486888-6890-47ac-a02d-b740868f143b",
        "type" => "application/vnd.openstack.image",
        :rel  => "alternate"
      }
    ],
    :disk_config            => "MANUAL",
    :id                     => "fe486888-6890-47ac-a02d-b740868f143b",
    :"OS-EXT-IMG-SIZE:size" => 8321922708,
    :name                   => "Windows Server 2012 R2 (base install without updates)",
    :created                => "2014-10-16T19:16:22Z",
    :minDisk                => 40,
    :progress               => 100,
    :minRam                 => 1024
  },
  {
    :state                  => "ACTIVE",
    :updated                => "2014-10-16T23:12:04Z",
    :links                  => [
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/images/c81a65a3-8217-4520-96de-1d9313ae3094",
        :rel  => "self"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/images/c81a65a3-8217-4520-96de-1d9313ae3094",
        :rel  => "bookmark"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/images/c81a65a3-8217-4520-96de-1d9313ae3094",
        "type" => "application/vnd.openstack.image",
        :rel  => "alternate"
      }
    ],
    :disk_config            => "MANUAL",
    :id                     => "c81a65a3-8217-4520-96de-1d9313ae3094",
    :"OS-EXT-IMG-SIZE:size" => 6944341570,
    :name                   => "Windows Server 2012 (base install without updates)",
    :created                => "2014-10-16T18:14:38Z",
    :minDisk                => 40,
    :progress               => 100,
    :minRam                 => 1024
  },
  {
    :state                  => "ACTIVE",
    :updated                => "2014-10-16T22:55:54Z",
    :links                  => [
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/v2/843739/images/b41c2705-f820-4b6f-8d32-d04b5f57a4f7",
        :rel  => "self"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/images/b41c2705-f820-4b6f-8d32-d04b5f57a4f7",
        :rel  => "bookmark"
      },
      {
        :href => "https://dfw.servers.api.rackspacecloud.com/843739/images/b41c2705-f820-4b6f-8d32-d04b5f57a4f7",
        "type" => "application/vnd.openstack.image",
        :rel  => "alternate"
      }
    ],
    :disk_config            => "MANUAL",
    :id                     => "b41c2705-f820-4b6f-8d32-d04b5f57a4f7",
    :"OS-EXT-IMG-SIZE:size" => 11521159761,
    :name                   => "Windows Server 2008 R2 SP1 + SQL Server 2012 SP1 Standard",
    :created                => "2014-10-16T16:04:28Z",
    :minDisk                => 40,
    :progress               => 100,
    :minRam                 => 2048
  }
]
```

## Example

```ruby
listen 'hipchat::message', room:'Factor', filter:'rackspace images ([a-zA-Z0-1])' do |hipchat|
  criteria = hipchat.matches[0]
  run 'rackspace::images:list', region:'dfw' do |images|
    image_list = images.map{ |i| "#{i.name} (#{i.id})" }.join(', ')
    run 'hipchat::send', room:'Factor', message: "Available images: #{image_list}"
  end
end
```

# rackspace::loadbalancer::list

## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region

## Output
```ruby
[
 {
   "name": "console",
   "id": 356337,
    "protocol": "HTTP",
    "port": 80,
    "algorithm": "RANDOM",
    "state": "ACTIVE",
    "timeout": 30,
    "created": "",
    "updated": "",
    "nodeCount": 1
  }
]
```

# rackspace::loadbalancer::get

## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region
id | false | The Load Balancer ID

## Output
```ruby
{
  "name": "console",
  "id": 356337,
  "protocol": "HTTP",
  "port": 80,
  "algorithm": "RANDOM",
  "status": "ACTIVE",
  "cluster": "",
  "nodes": [
    {
      "address": "10.182.130.27",
      "id": 724921,
      "type": "PRIMARY",
      "port": 80,
      "status": "ONLINE",
      "condition": "ENABLED"
    }
  ],
  "timeout": 30,
  "created": "",
  "sslTermination": "",
  "virtualIps": [
    {
      "address": "104.130.251.13",
      "id": 14967,
      "type": "PUBLIC",
      "ipVersion": "IPV4"
    },
    {
      "address": "2001:4800:7901:0000:49a2:e546:0000:0001",
      "id": 9277477,
      "type": "PUBLIC",
      "ipVersion": "IPV6"
    }
  ],
  "sourceAddresses": "",
  "httpsRedirect": false,
  "updated": "",
  "halfClosed": false,
  "connectionLogging": "",
  "contentCaching": ""
}
```

# rackspace::loadbalancer::delete

## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region
id | false | The Load Balancer ID


# rackspace::loadbalancer::create

## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region
id |  | The Load Balancer ID
name | | Name of the load balancer
protocol | 'HTTP' | The incoming protocol to use.
port | 80 | The TCP/IP Port to open for incoming connections
ip_type | 'PUBLIC' | Virtual IP Type. When your resources reside in the same region as your load balancer, devices are in close proximity to each other and can elect to take advantage of ServiceNet connectivity (by specifying 'SERVICENET') for free data transfer between services if desired.
algorithm | 'RANDOM' | Algorithm to use for route distribution.
wait | false | Should the call wait until the load balancer is created (or failed), or continue the workflow while this executes async. If subsequent code depends on this being live, then use 'true'.


# rackspace::loadbalancer::add_node
## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region
id |  | The Load Balancer ID
address |  | The host address, no port, scheme, or path
port | 80 | The TCP/IP Port to use
condition | 'ENABLED' | State of the node. Likely want to keep the default here.

# rackspace::loadbalancer::remove_node
## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region
load_balancer_id |  | The Load Balancer ID
id |  | Node ID


# rackspace::loadbalancer::nodes

## Input
ID | default | Description
-- | -------- | -----------
region | 'ord' | A string containing the three letter region
id |  | The Load Balancer ID

## Output

```ruby
[
  {
    "address": "10.182.130.27",
    "id": 724921,
    "type": "PRIMARY",
    "port": 80,
    "status": "ONLINE",
    "condition": "ENABLED"
  }
]
```