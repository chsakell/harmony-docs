# 💾 Databases

Harmony uses the following database providers:

* **SQL Server**: <mark style="color:orange;">Harmony</mark> database is used to store project management tool's **domain entities** such as workspaces, boards and cards. It also stores the user entities used for authentication and authorization. The applications that have a dependency to the Harmony database are <mark style="color:blue;">**Harmony.Server**</mark>, <mark style="color:blue;">**Harmony.Notifications**</mark> & <mark style="color:blue;">**Harmony.Automations**</mark>. A second SLQ Server database named <mark style="color:orange;">Harmony.Notifications</mark> is used to job related tasks by <mark style="color:blue;">**Harmony.Notifications**</mark> web only.&#x20;
* **MongoDB Server**: MongoDB schemaless database named <mark style="color:orange;">harmony\_automation</mark> is used for implementing flexible automations. The database contains the available automation template & the per board automation configurations. Configurations are written by the <mark style="color:blue;">Harmony.Server</mark> web app and being read by the <mark style="color:blue;">**Harmony.Automations**</mark>.
* **Redis**: Harmony is a scalable system and one of its main features is that syncs all updates across all connected clients. In order to support this <mark style="color:blue;">**Harmony.Server.SignalR**</mark>, the web application responsible to read messages from RabbitMQ and push them to all connected clients via websockets, uses [Redis Backplane](https://learn.microsoft.com/en-us/aspnet/core/signalr/redis-backplane?view=aspnetcore-8.0) for scaling out.

{% content-ref url="sql-server.md" %}
[sql-server.md](sql-server.md)
{% endcontent-ref %}

{% content-ref url="mongodb-server.md" %}
[mongodb-server.md](mongodb-server.md)
{% endcontent-ref %}

{% content-ref url="redis.md" %}
[redis.md](redis.md)
{% endcontent-ref %}
