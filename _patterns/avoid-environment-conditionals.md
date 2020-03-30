---
categories: Rails
name: Avoid environment based conditionals
---

It's common to query the current Rails environment when configuring code such an API client.

This can lead to duplicated conditional logic in various places so it's difficult to manage application configuration as it's scattered throughout the code base.

Instead we can manage custom application configuration, [the Rails way](https://guides.rubyonrails.org/configuring.html#custom-configuration). This has the added benefit of supporting inheritence and allowing the overriding of configuration.

## Example

### Bad

````ruby
hostname = if Rails.env.production?
  "biggerpockets"
elsif Rails.env.in? %w(staging review)
  ENV["HEROKU_APP_NAME"]
else
  ENV["DEFAULT_HOST"]
end
````

### Good

````ruby
hostname = Rails.configuration.x.hostname

# application.rb

config.x.hostname = ENV["DEFAULT_HOST"]

# production.rb

config.x.hostname = "biggerpockets"

# staging.rb

config.x.hostname = ENV["HEROKU_APP_NAME"]
````
