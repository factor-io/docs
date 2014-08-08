# Pushover

## Notify Action

### Credentials
- **user**:
- **password**:

### Fields:
- **message (required)**:
- **title (optional)**: (defualt: 'Factor.io')

### Response
    {
      'status':1,
      'request': '99fc262204305588f222c3445fd82364'
    }

### Example
    run 'pushover', 'notify', message: 'Whatup' do |notify_info|
      if notify_info['status'] == 1
        success "Message delivered" 
      else
        fail "Failed to deliver message"
      end
    end

# Notify Action
