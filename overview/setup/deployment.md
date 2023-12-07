# 🚀 Deployment

You need to deploy both Harmony.Server & Harmony.Notifications. Both of them are built with .NET Core so you can follow the same procedure.&#x20;

### Publishing the app

**1st command:** Open a terminal and navigate at the root of the <mark style="color:blue;">**Harmony.Server**</mark> project. Run the following command to create the release build:

```
dotnet publish -c Release
```

The produced build will be created inside a _**Harmony\Server\bin\Release\net7.0\publish**_ folder.

**2nd command:** Inside the _publish_ folder you can run the following command to start the project at the port **5000**:

```
dotnet run Harmony.Server.dll
```

Follow the same process to create the release build for <mark style="color:blue;">**Harmony.Notifications**</mark>.&#x20;

### Hosting Harmony

There are many options to deploy a .NET Core web application in production. Please advice Microsoft's [documentation](https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/?view=aspnetcore-7.0) for thorough details on hosting and deployment.

#### Following next: Make harmony your's

{% content-ref url="../buy-online.md" %}
[buy-online.md](../buy-online.md)
{% endcontent-ref %}

{% content-ref url="../../guide/workspaces/" %}
[workspaces](../../guide/workspaces/)
{% endcontent-ref %}
