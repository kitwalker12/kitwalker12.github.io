---
layout: post
title: Fixing Connection Tiemouts in Rails Worker Processes
date: 2015-08-20 10:51:00 -07:00
comments: true
categories: coding
tags: rails sidekiq sneakers rabbitmq
---

If you've been having trouble with the following error

```
could not obtain a database connection within 5.000 seconds (waited 5.000 seconds)
```

in your sidekiq or sneakers worker, there are 2 ways I came accross to mitigate it.

1. Your DB pool is probably lesser than the worker threads that are being spawned. Fix this by specifying the pool size to be equal to the number of threads in `config/database.yml`

```
production:
  url:  <%= ENV["DATABASE_URL"] %>
  pool: <%= ENV["DB_POOL"] || ENV['MAX_THREADS'] || 5 %>
```

2. Open and close the connection explicitly as the worker process may not be releasing the connection on its own after it finishes. I've seen this happen too many times with sneakers (a rabbitmq adapter)

```
ActiveRecord::Base.connection_pool.with_connection do
  # your code
end
```

Hope that helped.