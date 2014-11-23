Pushover is a simple notification service which you can use to send notifications to your mobile device.

## Authentication
You will need the Username (username) and API Key (api_key) from Pushover. The username can be found under the "Your User Key" section after you login to [Pushover](https://pushover.net). To get the API Key first you need to create an Application by followin the steps on "Register an Application". After you register, you will get an "API Key" for that applicaiton. The username and api_key are to be placed in the credentials.yml file.

**credentials.yml**

    pushover:
      username: NFOhPbj45iTHBXlZhqpw
      api_key: C9SwnAK55Px2BlgnTo

## Notify Action
The Notify Action (`pushover::notify`) sends messages to your Pushover clients.

### Parameters
- **message (required)**: The message to send to your device
- **title (optional)**: (defualt: 'Factor.io'): Optional title of the message

### Response
    {
      'status':1,
      'request': '99fc262204305588f222c3445fd82364'
    }

### Example
    listen 'github::push', repo:'skierkowski/hello' do |repo_info|
      run 'pushover::notify', message: "#{repo_info.repository.full_name} has new code" do |notify_info|
        if notify_info.status == 1
          success "Message delivered" 
        else
          fail "Failed to deliver message"
        end
      end
    end
