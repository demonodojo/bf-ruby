# BillForwardApiClient

This client library provides (via Ruby) easy access to the BillForward API.

## Installation

Add this line to your application's Gemfile and run `bundle`:

```ruby
gem 'bill_forward', :git => 'git@github.com:billforward/bf-ruby.git', :branch => 'master'
```

Or install this source as a local gem using:

```bash
    bundle
    gem build bill_forward.gemspec
    gem install bill_forward
```

## Usage
### Including the Gem

Once installed, require the BillForward gem:

```ruby
require 'bill_forward'
```

### Getting Credentials
You will need an API token. First log into your [Sandbox account](https://app-sandbox.billforward.net/login/#/) ([register](https://app-sandbox.billforward.net/register/#/) if necessary).

Then [generate an API token](https://app-sandbox.billforward.net/setup/#/personal/api-keys).

We support also client-id and client-secret authentication. For details, please [contact BillForward support](http://www.billforward.net/contact-us/).

### Connecting

Create a default Client. Requests will be sent using its credentials:

```ruby
my_client = BillForward::Client.new(
    :host =>      "API URL goes here",
    :api_token => "API token goes here"
)
BillForward::Client.default_client = my_client
```

### Invocation

##### Getting single entities:

e.g. Get Subscription by ID:

```ruby
subscription = BillForward::Subscription.get_by_id '3C39A79F-777E-4BDF-BDDC-221652F74E9D'
puts subscription
```

##### Accessing entity variables:

The entity can be accessed as a HashWithIndifferentAccess, or as an array.

```ruby
# The following are equivalent:
puts subscription.id
puts subscription['id']
puts subscription[:id]
```

##### Getting a list of entities:

e.g. List Accounts

```ruby
query_params = {
	'records'  => 3,
	'order_by' => 'created',
	'order'    => 'ASC'
}
accounts = BillForward::Account.get_all query_params
puts accounts
```

##### Creating an entity:

e.g. Create simple Account

```ruby
created_account = BillForward::Account.create
```

e.g. Create complex Account

```ruby
# Create an account with a profile (where the profile has addresses)
addresses = Array.new
addresses.push(
	BillForward::Address.new({
	'addressLine1' => 'address line 1',
    'addressLine2' => 'address line 2',
    'addressLine3' => 'address line 3',
    'city' => 'London',
    'province' => 'London',
    'country' => 'United Kingdom',
    'postcode' => 'SW1 1AS',
    'landline' => '02000000000',
    'primaryAddress' => true
	}))
profile = BillForward::Profile.new({
	'email' => 'always@testing.is.moe',
	'firstName' => 'Test',
	'addresses' => addresses
	})
account = BillForward::Account.new({
	'profile' => profile
	})
created_account = BillForward::Account.create account
puts created_account
```

##### Updating an entity

```ruby
gotten_account = BillForward::Account.get_by_id '908AF77A-0E5D-4D80-9B91-31EDE9962BF6'
gotten_account.profile.email = 'sometimes@testing.is.moe'
updated_account = gotten_account.save() # or: gotten_account.profile.save()
puts updated_account
```
