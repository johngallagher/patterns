---
categories: Ruby
name: Don't lock gems
---

Unless we have a good reason (for example, incompatibility) we should avoid automatically locking the version of a gem we are using.

This means that when we run `bundle update` or Depfu runs, we will be upgraded to the latest gem version. CI will fail if there are breaking changes and we should address any issues there and then. This helps ensure we stay up to date and any security patches are brought in immediately.

## Bad

```ruby
# Gemfile

gem "nokogiri", "~> 1.10.3"
```

## Good

```ruby
# Gemfile

gem "nokogiri"
```
