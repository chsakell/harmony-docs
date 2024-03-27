# 🚢 Deployment

When you are ready to deploy Harmony to a different environment _(e.g. staging or production)_, you need to setup the deployments for the following dependencies & apps:

### Dependencies to be installed

* [x] SQL Server
* [x] MongoDB Server
* [x] RabbitMQ
* [x] Redis instance _(optionally, required only if you run multiple instances of Harmony.SignalR)_

The dependencies can be installed either on premise infrastructure or use cloud/managed providers. In either case, you will need to configure their connection strings in all corresponding settings files _(basically **appsettings.\<environment>.json**)_.\
There isn't better documentation for installing them than their official pages.

### Applications to deploy

* [x] Harmony.Api
* [x] Harmony.SignalR
* [x] Harmony.Automations
* [x] Harmony.ApiGateway
* [x] Harmony.Notifications
* [x] Harmony.Client

All the above applications have been built with .NET 8.0 so you can follow Microsoft's [documentation](https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/?view=aspnetcore-8.0) for thorough details on hosting and deployment.&#x20;

{% hint style="warning" %}
**Attention for **<mark style="color:orange;">**Harmony.Client**</mark>

The front end of Harmony is a **Standalone Blazor wasm** web app which means it doesn't have any dependency on .NET Core hosting. That's why you may have noticed that nginx is used to serve the app in the docker containers.&#x20;

You are free to use any host of your preference such as nginx or IIS to host <mark style="color:orange;">Harmony.Client</mark>, but always treat it as a **static website**.
{% endhint %}

### Configurations - things to consider

* **ApiGateway**: The first thing you need to configure is the **ocelot.json** _DownstreamHostAndPorts_ property to match your environment URLs. All you have to do is change _localhost_ & _port_ property to the ones that reflects your production environment. Things are easier if you use docker, since the **ocelot.docker.json** file is used instead, which uses the services names rather than fixed URLs. In any case, if you use docker for production, you may have to configure that file accordingly. \
  Of course before configuring these settings you must have already configured & deploy Harmony.Api, Harmony.Automations & Harmony.Signalr applications.
* **Services**: For all web apps existing in the _Services_ solution folder, configure all the properties in the _appsettings.\<environment>.json_ file. Each app may have different dependencies, e.g. RabbitMq, MongoDb or SQL Server. Fill the correct connection strings for all of them to match your environment.
* **Harmony.Client**: The only property you need to configure is the _gatewayUrl_ property existing in the _**www/appsettings.\<environment>.json**_ file. This is the hosting URL for the <mark style="color:blue;">Harmony.ApiGateway</mark> application which should be publicly accessible.&#x20;

{% hint style="success" %}
All these settings are already configured for **local** development & debugging either directly from code or using docker containers so feel free to fully understand them before proceeding to installing Harmony to a new environment.
{% endhint %}

#### Following next: Debugging and running Harmony locally

{% content-ref url="before-running.md" %}
[before-running.md](before-running.md)
{% endcontent-ref %}

{% content-ref url="../pricing.md" %}
[pricing.md](../pricing.md)
{% endcontent-ref %}

{% content-ref url="../../guide/workspaces/" %}
[workspaces](../../guide/workspaces/)
{% endcontent-ref %}
