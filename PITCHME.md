<!-- $theme: default -->
<!-- page_number: true -->

### Caching with Rails
*Using Redis to Cache Data*

---

### Scenario:
*- 3 party API blocks identical requests in seconds.*
![screenshot](https://github.com/bikonchou/Sharing/blob/caching_with_rails/assets/images/screenshot1.png?raw=true)

---

### Just few steps to go:
*- install Redis*
*- install 'redis-rails'*
```ruby
# Gemfile
gem 'redis-rails'
```
*- config environment.rb*
```ruby
# :redis_store, 'redis:://Host/Database/Namespace', { options }
config.cache_store = :redis_store, 'redis://localhost:6379/0/cache', { expires_in: 90.minutes }
```

---

### My first commit
```ruby
# config/initializers/redis.rb
$redis = Redis::Namespace.new("cache", redis: Redis.new)

# Set cache
3party_response = request_3party_api
$redis.set(cache_key, 3party_response)

# Get cache
$redis.get(cache_key)
# => 3party_response
```

---

### Better not to
*- Set options in global config*
```ruby
config.cache_store = :redis_store, 'redis://localhost:6379/0/'
```
### Better to 
*- Use Rails default cache object*
```ruby
Rails.cache.write(cache_key, value, expire_in: 5.hours)
Rails.cache.read(cache_key) # => value
```
```ruby
# Can Combine above with 'fetch'
Rails.cache.fetch(cache_key, namespace: 'cache', expire_in: 5.hours) do
  request_3party_api
end
```


---

### Traps I've fallen (1)
*- TypeError*
*- Only accept Basic data format like String, Array, Hash*
```ruby
Rails.cache.fetch(cache_key, options) do
  response = request_3party_api
  # response.class => HTTParty
  response.to_json
end
```
---

### Traps I've fallen (2)
*- Redis NOAUTH*
*- If you've set password in Redis, remember to send it in connection*

```ruby
config.cache_store = :redis_store, "redis://#{SecretSettings.redis.host}:#{SecretSettings.redis.port}/0/", { password: SecretSettings.redis.password, raise_error: false }
```

---

