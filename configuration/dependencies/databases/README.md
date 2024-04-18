# 💾 Databases

Harmony uses the following database providers:

* **SQL Server**: <mark style="color:orange;">Harmony</mark> database is used to store project management tool's **domain entities** such as workspaces, boards and cards. It also stores the user entities used for authentication and authorization. The only application that has a dependency to the Harmony database is the <mark style="color:blue;">**Harmony.Api**</mark>**.**\
  <mark style="color:blue;">**Harmony.Notifications**</mark> & <mark style="color:blue;">**Harmony.Automations**</mark> apps use their own databases, _Harmony.Notifications.Jobs_ & _Harmony.Automations.Jobs_ respectively for HangFire jobs.&#x20;
* **MongoDB Server**: MongoDB schema-less database named <mark style="color:orange;">harmony\_automation</mark> is used for implementing flexible automations. The database contains the available automation template & the per board automation configurations. The only application than can directly access the MongoDB database is the <mark style="color:blue;">**Harmony.Automations**</mark>.
* **Redis**: Harmony is a scalable system and one of its main features is that syncs all updates across all connected clients. In order to support this <mark style="color:blue;">**Harmony.SignalR**</mark>, the web application responsible to read messages from RabbitMQ and push them to all connected clients via WebSocket, uses [Redis Backplane](https://learn.microsoft.com/en-us/aspnet/core/signalr/redis-backplane?view=aspnetcore-8.0) for scaling out.

{% content-ref url="sql-server.md" %}
[sql-server.md](sql-server.md)
{% endcontent-ref %}

{% content-ref url="mongodb-server.md" %}
[mongodb-server.md](mongodb-server.md)
{% endcontent-ref %}

{% content-ref url="redis.md" %}
[redis.md](redis.md)
{% endcontent-ref %}
