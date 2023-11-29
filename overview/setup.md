# ⚙ Setup

Harmony is a web application built with **.NET Core** which means it's cross platform and can be deployed anywhere _(Windows, Linux, Mac)_. It also requires an **SQL Server** database which can be installed on Windows or [Linux](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-setup?view=sql-server-ver16#supportedplatforms).&#x20;

### Database connection string

Configure the SQL Server's connection string existing in the <mark style="color:blue;">**appsettings.json**</mark> file at the root of the **Harmony.Server** project to point to your SQL Server instance.

```json
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  }
```

### Database migrations

You can run the database migrations either manually or let the project run them for you during startup.

#### Run migrations through <mark style="color:blue;">Visual Studio</mark>

When running migrations through Visual Studio, open the `Package Manager Console` and set the `Default project` to **src\Infrastructure\Harmony.Persistence**.

Run the following command to create the database:

```powershell
Update-Database -Context HarmonyContext -StartUpProject Harmony.Server -v
```

<figure><img src="../.gitbook/assets/visual-studio-migrations-update-database.png" alt=""><figcaption><p>Create database migration through Visual Studio</p></figcaption></figure>

{% hint style="warning" %}
Migrations command require that you have previously setup your database connection string properly.
{% endhint %}

In case you decide to **create** a new migration, follow the same procedure by replacing the command with the following:

```powershell
Add-Migration MyCustomMigrationName -Context HarmonyContext -StartUpProject Harmony.Server -v// Some code
```

#### Run migrations outside <mark style="color:blue;">Visual Studio</mark> using a command line

You can run database migrations from a command line as well. First make sure you have installed [EF Core tools](https://learn.microsoft.com/en-us/ef/core/cli/dotnet).

```powershell
dotnet tool install --global dotnet-ef
```

1. Open a terminal and navigate at the root of the **Harmony.Persistence** project, where the <mark style="color:blue;">HarmonyContext</mark> database context class exists.
2. Run the **dotnet ef** command to create the database

```powershell
dotnet ef database update --context HarmonyContext --startup-project "../../Web/Harmony/Server/Harmony.Server.csproj"
```

<figure><img src="../.gitbook/assets/command-line-update-database.png" alt=""><figcaption><p>Create database migration through command line</p></figcaption></figure>

{% hint style="info" %}
Just a reminder here: It's **optional** to run the migrations by yourself because they will run by default at startup. At a later release, this will be active only in **debug** mode.
{% endhint %}

To disable the automatic migrations remove the following line from the <mark style="color:blue;">ApplicationBuilderExtensions</mark> class.

```csharp
harmonyContext.Database.Migrate();
```

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

### Publishing the app

{% hint style="warning" %}
There are currently two issues that you need to **fix** when running the following commands. `Both issues have been fixed and will be released in the next release version.`

1. **Before running the 1st command:** Correct a typo issue which exists in the <mark style="color:blue;">ServiceCollectionExtensions</mark> class of the **Harmony.Server** project. This build error exists only for release profile. Simply remove the <mark style="color:red;">\[</mark> character from the following line:\
   \
   `var result = JsonSerializer.Serialize(Result.Fail(`<mark style="color:orange;">`[`</mark>`"An unhandled error has occurred."));`
2. **Before running the 2nd command**: Create manually a folder named _**Files**_ inside the published folder
{% endhint %}

**1st command:** Open a terminal and navigate at the root of the **Harmony.Server** project. Run the following command to create the release build:

```
dotnet publish -c Release
```

The produced build will be created inside a _**Harmony\Server\bin\Release\net7.0\publish**_ folder.

**2nd command:** Inside the _publish_ folder you can run the following command to start the project at the port **5000**:

```
dotnet run Harmony.Server.dll
```

### Hosting Harmony

There are many options to deploy a .NET Core web application in production. Please advice Microsoft's [documentation](https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/?view=aspnetcore-7.0) for thorough details on hosting and deployment.

#### Read next - Buy Harmony

{% content-ref url="buy-online.md" %}
[buy-online.md](buy-online.md)
{% endcontent-ref %}
