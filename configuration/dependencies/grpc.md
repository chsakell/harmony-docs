# 🚀 gRPC

gRPC is a high-performance Remote Procedure Call (RPC) framework and it's being used for internal microservice communication. More specifically:

* Harmony.Api exposes gRPC to other services in order to access the harmony database
* Harmony.Automations exposes gRPC to Harmony.Api in order to access the MongoDB automations database.
* Harmony.Integrations.SourceControl exposes gRPC to Harmony.Api for source control data (`AppEndpointConfiguration:SourceControlEndpoint`, e.g. `https://localhost:7113/`).

### Configurations

Each application that consumes a gRPC service needs to know the endpoint of the application that exposes the service.

#### Harmony.Api

Harmony.Api consumes the gRPC service exposed by the Harmony.Automations. Set the AppEndpointConfiguration in the **appsettings.json**.

```json
"AppEndpointConfiguration": {
    "AutomationEndpoint": "https://localhost:7277/",
    "FrontendUrl": "https://localhost:7096"
  },
```

The AutomationEndpoint is the actuall Harmony.Automations endpoint used when the app is started.

#### Harmony.Automations & Harmony.Notifications

Both of <mark style="color:blue;">Harmony.Automations</mark> & <mark style="color:blue;">Harmony.Notification</mark>s services consume the gRPC service exposed by the <mark style="color:blue;">Harmony.Api</mark>. Set the _AppEndpointConfiguration_ in their **appsettings.json** file.

```json
  "AppEndpointConfiguration": {
    "HarmonyApiEndpoint": "https://localhost:7097/"
  },
```

{% hint style="info" %}
With [.NET Aspire](../aspire.md) you don't configure these values. The AppHost injects `AppEndpointConfiguration__AutomationEndpoint`, `__SourceControlEndpoint`, `__HarmonyApiEndpoint` and `__FrontendUrl` from the actual endpoints of the running services. gRPC calls show up as spans in the distributed traces of the Aspire dashboard.
{% endhint %}

{% hint style="warning" %}
### Production

In case you deploy Harmony on a production environment don't forget to set the corresponding _AppEndpointConfiguration_ configurations in all the appsettings.Production.json files
{% endhint %}

#### Read next - Configure a search engine (optional)

{% content-ref url="search-engine.md" %}
[search-engine.md](search-engine.md)
{% endcontent-ref %}
