# 🏃♂ Debugging

### Running the app through Visual Studio

In case you want to run or debug the application from Visual Studio, make sure to set the **Harmony.Server** project as the startup or simply right click to it and select **Debug -> Start new instance**.

### Running the app through command line

In order to run Harmony through command line, open a terminal and navigate at the root of the **Harmony.Server** project. Make sure you have the [dotnet runtime](https://dotnet.microsoft.com/en-us/download) or SDK installed _(currently .NET 7.0 is required but .NET 8.0 should work as well)_ and run the following command:

```
dotnet run
```

By default the app will run at [http://localhost:5181/](http://localhost:5181/). In case you want to change this change **5181** to what port you want at the **launchSettings.json** file for the http profile. The following example set the port to **5000**.

```json
      "http": {
        "commandName": "Project",
        "dotnetRunMessages": true,
        "launchBrowser": true,
        "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}",
        "applicationUrl": "http://localhost:5000",
        "environmentVariables": {
          "ASPNETCORE_ENVIRONMENT": "Development"
        }
      },
```

