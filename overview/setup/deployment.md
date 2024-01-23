# 🚀 Deployment

When you are ready to deploy Harmony to a production environment you need to setup the deployments for the following components & apps:

#### Components

* [x] SQL Server
* [x] MongoDB Server
* [x] RabbitMQ
* [x] Redis instance

#### Applications

* [x] Harmony.Server
* [x] Harmony.Server.SignalR
* [x] Harmony.Notifications
* [x] Harmony.Automations

After installing the required components in the production environment, go ahead and configure all the **appsettings.Production.json** files in the applications to reflect your production setup.

{% hint style="warning" %}
Don't forget to set the <mark style="color:orange;">signalrHostUrl</mark> property in the **appsettings.Production.json** file existing in the **Harmony.Client** project _(www folder)_ which configures the **Harmony.SignalR** project's host URL for clients!
{% endhint %}

### Publishing the app

In case you build the artifacts manually, run the following command for all web applications after navigating at their root's folder.

**1st command:** Open a terminal and navigate at the root of a web app's project. Run the following command to create the release build:

```
dotnet publish -c Release
```

The produced build will be created inside a _**\<path-to-project>\bin\Release\net8.0\publish**_ folder.

**2nd command:** Inside the _publish_ folder you can run the following command to start the app:

```
dotnet run <project-name>.dll
```

### Hosting Harmony

There are many options to deploy a .NET Core web application in production. Please advice Microsoft's [documentation](https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/?view=aspnetcore-8.0) for thorough details on hosting and deployment.

#### Following next: Debugging and running Harmony locally

{% content-ref url="before-running.md" %}
[before-running.md](before-running.md)
{% endcontent-ref %}

{% content-ref url="../buy-online.md" %}
[buy-online.md](../buy-online.md)
{% endcontent-ref %}

{% content-ref url="../../guide/workspaces/" %}
[workspaces](../../guide/workspaces/)
{% endcontent-ref %}
