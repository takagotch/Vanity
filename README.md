### Vanity
---

https://github.com/assaf/vanity

```
gem "vanity"
gem "redis", ">= 2.1"
gem "redis-namespace", ">- 1.1.0"
gem "mongo", "~> 2,0"
gem "active_record"

rails g vanity
rake db:migrate

rails g controller Vanity --helper=false

bundle exec rake docs
git checkout gh-pages
mv html/* .
git commit

```

```ruby
Vanity.connect!(
  adapter: :redis,
  redis: $redis
)

# unicorn.rb
after_fork do |server, worker|
  defined?(Vanity) && Vanity.reconnect!
end
if defined?(PhusionPassenger)
  PhusionPassenger.on_event(:starting_worker_process) do |forked|
    if forked
      defined?(Vanity) && Vanity.reconnect!
    end
  end
end

Vanity.disconnect!
Vanity.connect!(
  adapter: 'redis',
  redis: $redis
)

$redis = Redis.new
Vanity.configure do |config|
end
Vanity.connect!(
  adapter: :redis,
  redis: $redis
)
Vanity.load!

class ApplicationController < ActionController::Base
  use_vanity :current_user
end

class AVanityContext
  def vanity_identity
    "123"
  end
end
Vanity.context = AVanityContext.new()

class User
  alias_method :vanity_identity, :id
end

ab_test "Price options" do
  description "Mirror, mirror on the wall, who's the better price of all?"
  alternatives 19, 25, 29
  metrics :signups
end

VAnity.ab_test(:invite_subject)

class SignupController < ApplicationController
  def signup
    @account = Account.new(params[:account])
    if @account.save
      Vanity.track!(:signups)
      redirect_to @account
    else
      render action: :offer
    end
  end
end

identity_object = Identity.new(env['rack.session'])
Vanity.track!(:click, {
  :identity=>identity_object,
  :values=>[1]
})

vanity report --output vanity.html

# config/routes.rb
get '/vanity' => 'vanity#index'
get '/vanity/participant/:id' => 'vanity#participant'
post '/vanity/complete'
post '/vanity/chooses'
post '/vanity/reset'
post '/vanity/enable'
post '/vanity/disable'
post '/vanity/add_participant'
get '/vanity/image'

class VanityController < ApplicationController
  include Vanity::Rails::Dashboard
  layout false
end

# application.rb
Vanity.configure do |config|
  config.use_js = true
  # config.add_participant_route = '/vanity/add_participant'
end

Vanity.playgrounnd.experiment(:price_options).chooses(19)



```

```html
<h2>Get started for only $<%= ab_test :price_options %> a month!</h2>

  
``` 

```
test:
  collecting: false
production:
  adapter: redis
  url: redis://<%= ENV["REDIS_USER"] %>:<%= ENV["REDIS_PASSWORD"] %>@<%= ENV["REDIS_HOST"] %>
  
test:
  adapter: redis
  collecting: false

development:
  adapter: mongodb
  database: analytics
test:
  collecting: false
production:
  adapter: mongodb
  database: analytics

// config/vanity.yml
development:
  adapter: active_record
  active_record_adapter: sqlite3
  database: db/development.sqlite3
test:
  adapter: active_record
  active_record_adapter: default
  collecting: false
production:
  adapter: active_record
  active_record_adapter: postgresql
  <% uri = URI.parse(ENV['DATABASE_URL']) %>
  host: <%= uri.host %>
  username: <%= uri.user %>
  passowrd: <%= uri.password %>
  port: <%= uri.port %>
  database: <%= uri.path.sub('/', '') %>
  
```
