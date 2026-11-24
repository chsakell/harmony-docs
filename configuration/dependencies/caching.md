---
description: Caching dramatically improves application's performance
---

# 🗃️ Caching

There are two types of caching available, _**InMemory**_ & _**Redis**_.

* In memory caching is used by default
* Redis should be used instead in production environments where multiple instances of the same application might exist.

### Configuration

Configure the **CacheProvider** _appsettings.json_ property as follow:

* **In Memory Caching**: _InMemory_ or  empty string or non existing property:
* **Redis Caching**: Redis

**Examples**

```json
"CacheProvider": "InMemory"
```

```json
"CacheProvider": "Redis",
"RedisConnectionString": "127.0.0.1:6379"
```

#### Notes

* With Redis caching, you also need to configure the **RedisConnectionString** property. Any StackExchange.Redis connection string works, e.g. `localhost:6379,password=MyPassword`.
* Configure the caching for <mark style="color:blue;">Harmony.Api</mark> & <mark style="color:blue;">Harmony.Automations</mark> projects

{% hint style="info" %}
[.NET Aspire](../aspire.md), docker compose and Kubernetes set `CacheProvider=Redis` and the matching `RedisConnectionString` on Harmony.Api & Harmony.Automations. Running the projects without them keeps the default in-memory cache.
{% endhint %}
