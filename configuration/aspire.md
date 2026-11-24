---
description: Run the whole Harmony stack locally with one command, with distributed tracing out of the box
---

# 🌟 .NET Aspire

[.NET Aspire](https://learn.microsoft.com/en-us/dotnet/aspire/get-started/aspire-overview) is the **recommended way to run Harmony locally**. A single project, <mark style="color:blue;">**Harmony.AppHost**</mark>, starts every dependency as a container (SQL Server or PostgreSQL, MongoDB, RabbitMQ, Redis) together with all the Harmony services. It wires their configuration automatically, and collects **traces, metrics and logs** of every service in the Aspire dashboard.

You don't need to install SQL Server, MongoDB, RabbitMQ or Redis on your machine, configure connection strings, or set up multiple startup projects.

{% hint style="success" %}
Databases survive restarts. All containers use **persistent** lifetime and **named Docker volumes**, so your workspaces, boards and cards are still there the next time you run the AppHost, even if the containers are removed.
{% endhint %}

### Prerequisites

* [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/), **running**
* A trusted HTTPS development certificate: `dotnet dev-certs https --trust`
* Optionally Visual Studio 2026, VS Code with the C# Dev Kit, Rider, or the [Aspire CLI](https://learn.microsoft.com/en-us/dotnet/aspire/cli/install)

The solution contains two Aspire projects under **src/Aspire**:

| Project | Purpose |
| ------- | ------- |
| <mark style="color:blue;">**Harmony.AppHost**</mark> | Orchestrates the containers and the Harmony services. This is your **only startup project**. |
| <mark style="color:blue;">**Harmony.ServiceDefaults**</mark> | OpenTelemetry (traces, metrics, logs), service discovery and HTTP resilience. Referenced by Harmony.Api, Automations, Notifications, SignalR, Integrations.SourceControl and ApiGateway. |

### Run

{% tabs %}
{% tab title="Visual Studio / Rider" %}
Set **Harmony.AppHost** as the startup project and press **F5**. Pick a launch profile from the run button:

* `https`: SQL Server container (default)
* `https-postgres`: PostgreSQL container
* `https-local-sqlserver`: your own SQL Server instance, e.g. `localhost\SQLEXPRESS`
{% endtab %}

{% tab title="Command line" %}
```powershell
# from the repository root
aspire run

# or, without the Aspire CLI
dotnet run --project src/Aspire/Harmony.AppHost
dotnet run --project src/Aspire/Harmony.AppHost --launch-profile https-postgres
```
{% endtab %}
{% endtabs %}

The console prints the dashboard login link, for example `https://localhost:17180/login?t=...`. When every resource is **Running / Healthy**, open Harmony at [https://localhost:7096](https://localhost:7096) and sign in with one of the default users.

{% hint style="info" %}
The first run takes a few minutes because Docker pulls the container images (SQL Server alone is about 1.5 GB). Later runs reuse the images and the running containers, so they start in seconds.
{% endhint %}

### What starts

| Resource | Host port | Volume | Notes |
| -------- | --------- | ------ | ----- |
| **sql** (SQL Server 2022) | 14333 | `harmony-sqlserver-data` | Databases `Harmony`, `Harmony.Automations.Jobs`, `Harmony.Notifications.Jobs` |
| **postgres** _(PostgreSQL provider only)_ | 54333 | `harmony-postgres-data` | Same three databases, plus pgAdmin |
| **mongodb** | 27018 | `harmony-mongodb-data` | Plus Mongo Express |
| **rabbitmq** | 5673 (management UI 15673) | `harmony-rabbitmq-data` | User `harmony` |
| **redis** | 6380 | `harmony-redis-data` | Cache and SignalR backplane, plus RedisInsight |
| **elasticsearch** / **kibana** _(optional)_ | 9200 / 5601 | `harmony-elasticsearch-data` | See [Elasticsearch & Kibana](#elasticsearch-and-kibana) |
| **api** | 7097 | | Harmony.Api |
| **automations** | 7277 | | Harmony.Automations |
| **notifications** | 7121 | | Harmony.Notifications |
| **signalr** | 7262 | | Harmony.SignalR |
| **sourcecontrol** | 7113 | | Harmony.Integrations.SourceControl |
| **gateway** | 7108 | | Harmony.ApiGateway (Ocelot) |
| **client** | 7096 | | Harmony.Client (Blazor WebAssembly) |

The services keep the same ports as before, so **ocelot.json** and the client's **gatewayUrl** don't change.

The AppHost injects all configuration as environment variables in the shape the services already read, overriding **appsettings.json**:

* `ConnectionStrings__HarmonyConnection` / `ConnectionStrings__HarmonyJobsConnection` and `DatabaseProvider`
* `BrokerConfiguration__Host`, `__Port`, `__Username`, `__Password`, `__VirtualHost`
* `MongoDB__ConnectionURI`
* `RedisConnectionString`, plus `CacheProvider=Redis` for Harmony.Api & Harmony.Automations. In Harmony.SignalR it turns on the Redis backplane.
* `AppEndpointConfiguration__*` (Api, Automations and SourceControl endpoints, FrontendUrl)
* `ElasticConfiguration__Uri`, which is empty unless Elasticsearch is enabled

#### Startup order

The AppHost starts the services in dependency order using `WaitFor`. A service starts only after each of its dependencies is running **and** healthy. Containers report healthy through their Aspire health checks, and Harmony services through their `/healthz/ready` endpoint. While a service waits, the dashboard shows it as <mark style="color:orange;">**Waiting**</mark>.

| Service                   | Waits for                                                  | Why                                                                  |
| ------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------- |
| Harmony.Api               | Harmony database, RabbitMQ, Redis                          | Migrates & seeds the database, publishes integration events, caching |
| Harmony.SourceControl     | MongoDB, RabbitMQ                                          | Repositories & readiness check, publishes events                     |
| Harmony.Automations       | Automations jobs database, MongoDB, RabbitMQ, Redis, Api   | Hangfire, seeds MongoDB, consumes events, caching, gRPC calls to Api |
| Harmony.Notifications     | Notifications jobs database, RabbitMQ, Api                 | Hangfire, consumes events, gRPC calls to Api                         |
| Harmony.SignalR           | RabbitMQ, Redis                                            | Consumes events, Redis backplane                                     |
| Harmony.ApiGateway        | Api, Automations, SourceControl, SignalR                   | Ocelot downstream routes                                             |
| Harmony.Client            | ApiGateway                                                 | `gatewayUrl`                                                         |

{% hint style="info" %}
Harmony.Api and Harmony.Automations / SourceControl call each other over gRPC, so only one direction can be a startup dependency. The Api starts first because it owns the Harmony database. With `LocalSqlServer` your own SQL Server isn't an Aspire resource, so the services don't wait for it.
{% endhint %}

### The dashboard

<mark style="color:blue;">**Resources**</mark> lists every container and service with its state, endpoints, environment variables and connection strings. From here you open the RabbitMQ management UI, Mongo Express, RedisInsight or pgAdmin, and stop or restart a single service.

<mark style="color:blue;">**Console logs**</mark> shows the raw stdout of each resource.

<mark style="color:blue;">**Structured logs**</mark> shows the Serilog events of all services, searchable and filterable. Each event links to the trace it belongs to.

<mark style="color:blue;">**Traces**</mark> shows end-to-end distributed traces. For example, moving a card produces **one** trace that spans:

1. `gateway`: the request from the browser, forwarded by Ocelot
2. `api`: the HTTP request, with the EF Core SQL queries
3. `api`: the RabbitMQ publish to the `harmony_notifications` exchange
4. `signalr`, `automations`, `notifications`: the RabbitMQ consume, whose trace context is propagated through the message headers
5. `automations` / `notifications`: gRPC calls back to `api`, and MongoDB commands

<mark style="color:blue;">**Metrics**</mark> shows ASP.NET Core, HttpClient and .NET runtime metrics per service.

{% hint style="info" %}
What is traced: ASP.NET Core requests, outgoing HTTP (including gRPC and Ocelot downstream calls), EF Core (SQL Server & PostgreSQL), RabbitMQ publish and consume, MongoDB commands, and SignalR hub invocations. Redis and Hangfire are not traced.
{% endhint %}

### Choose the database provider

The provider is set with the `Harmony:DatabaseProvider` setting of the AppHost. You can pick a launch profile, edit **src/Aspire/Harmony.AppHost/appsettings.json**, or set the `Harmony__DatabaseProvider` environment variable.

| Value | Database |
| ----- | -------- |
| `SqlServer` _(default)_ | SQL Server container |
| `PostgreSQL` | PostgreSQL container + pgAdmin |
| `LocalSqlServer` | Your own SQL Server instance |

#### Use your local SQL Server

With `LocalSqlServer` (launch profile `https-local-sqlserver`), no database container starts. The AppHost passes the connection strings `harmony-local`, `automations-jobs-local` and `notifications-jobs-local` from its own configuration to the services. By default they point to `localhost\SQLEXPRESS` with Windows authentication. Override them in the AppHost user secrets:

```powershell
dotnet user-secrets set "ConnectionStrings:harmony-local" "Server=.;Database=Harmony;Integrated Security=True;TrustServerCertificate=True" --project src/Aspire/Harmony.AppHost
dotnet user-secrets set "ConnectionStrings:automations-jobs-local" "Server=.;Database=Harmony.Automations.Jobs;Integrated Security=True;TrustServerCertificate=True" --project src/Aspire/Harmony.AppHost
dotnet user-secrets set "ConnectionStrings:notifications-jobs-local" "Server=.;Database=Harmony.Notifications.Jobs;Integrated Security=True;TrustServerCertificate=True" --project src/Aspire/Harmony.AppHost
```

### Persistence, passwords and reset

The passwords of SQL Server, PostgreSQL, MongoDB, RabbitMQ and Redis are generated on the first run. They are stored in the AppHost **user secrets** as `Parameters:sql-password`, `Parameters:postgres-password`, `Parameters:mongodb-password`, `Parameters:rabbitmq-password` and `Parameters:redis-password`. To see them, run:

```powershell
dotnet user-secrets list --project src/Aspire/Harmony.AppHost
```

You can also see them in the resource details in the dashboard.

{% hint style="warning" %}
Keep these values. The data volumes are initialized with them on the first run. If you delete or change a password while its volume still exists, the database container won't accept the new password.
{% endhint %}

To start from scratch:

1. Stop the AppHost.
2. Remove the persistent containers. Aspire names them `<resource>-<suffix>`, e.g. `sql-2d9aa915`, `mongodb-2d9aa915` or `rabbitmq-2d9aa915`. Use `docker ps -a` to find them, then `docker rm -f <names>`, or remove them from Docker Desktop.
3. Remove the volumes you want to reset, e.g. `docker volume rm harmony-sqlserver-data harmony-mongodb-data harmony-rabbitmq-data harmony-redis-data`.
4. Optionally remove the matching `Parameters:*` user secret: `dotnet user-secrets remove "Parameters:sql-password" --project src/Aspire/Harmony.AppHost`.

### Connect with your tools

| Tool | Connection |
| ---- | ---------- |
| SSMS / Azure Data Studio | Server `localhost,14333`, SQL authentication, user `sa`, password `Parameters:sql-password` |
| pgAdmin | Opened from the **postgres** resource in the dashboard; already connected |
| Mongo Express | Opened from the **mongodb** resource in the dashboard |
| MongoDB Compass | The **mongodb** connection string from the dashboard (port 27018) |
| RabbitMQ management | [http://localhost:15673](http://localhost:15673), user `harmony`, password `Parameters:rabbitmq-password` |
| RedisInsight | Opened from the **redis** resource in the dashboard |

### Elasticsearch and Kibana

Logs always show up in the dashboard, so Elasticsearch is **off** by default. To also run Elasticsearch & Kibana and turn on the Serilog Elasticsearch sink, set the following in **src/Aspire/Harmony.AppHost/appsettings.json** or in user secrets:

```json
"Harmony": {
  "EnableElasticsearch": true
}
```

Kibana is then available at [http://localhost:5601](http://localhost:5601). See [distributed-logging.md](docker/distributed-logging.md "mention") for how to set up the index pattern.

### Troubleshooting

* **A port is already in use**: stop anything else that listens on the Harmony ports (7096, 7097, 7108, 7113, 7121, 7262, 7277), such as Visual Studio running the old multi-startup profile or `docker compose`. The same applies to 14333, 54333, 27018, 5673, 15673 and 6380.
* **Containers don't start**: make sure Docker Desktop is running. The first run can take several minutes while images are pulled. Check the **Console logs** of the resource.
* **A database rejects the login after a reset**: the volume still has the old password. See [Persistence, passwords and reset](#persistence-passwords-and-reset).
* **Browser certificate warnings**: run `dotnet dev-certs https --trust` and restart the browser.

{% content-ref url="before-running.md" %}
[before-running.md](before-running.md)
{% endcontent-ref %}

{% content-ref url="docker/" %}
[docker](docker/)
{% endcontent-ref %}
