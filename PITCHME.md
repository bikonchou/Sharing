<!-- $theme: default -->

<!-- page_number: true -->

# Caching with Rails
*Using Redis*

---

## Scenario:
## *3 party API blocks same requests in seconds.*

![screenshot](/Users/bikonchou/Desktop/Cache%20Sharing/screenshot1.png)

---

# Just few steps to go:
## - install Redis
## - install 'redis-rails'
```ruby
# Gemfile
gem 'redis-rails'
```
## - config environment.rb
```ruby
# :redis_store, 'redis:://Host/Database/Namespace', { options }
config.cache_store = :redis_store, 'redis://localhost:6379/0/cache', { expires_in: 90.minutes }
```

---

# 