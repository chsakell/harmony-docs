---
description: Install and configure all the required dependencies for your local environment
---

# 📀 Installations

As you can see in the architecture diagram, Harmony has a few dependencies that need to be installed and configured before running. These are the followings:

* SQL Server
* MongoDB
* RabbitMQ
* Redis _(optional, only install it on a production environment if you have more than 1 **Harmony.SignalR** instances)_

<figure><img src="../../.gitbook/assets/harmony-architecture.gif" alt=""><figcaption></figcaption></figure>

In case you don't have any experience with installing/running and configuring this kind of software, the following sections will guide you through installing and configuring all these dependencies on your local machine.

### Install .NET 8.0

If you are going to use Visual Studio for development then the only thing you need to do is to install [Visual Studio 2022](https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022). Only the **ASP.NET and web development** workload is needed.

{% embed url="https://www.youtube.com/watch?v=SViilF85ues" %}
Install Visual Studio 2022
{% endembed %}

In case you don't want to use Visual Studio for development, you can manual install [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) as shown on the following video.

{% embed url="https://www.youtube.com/watch?v=0YvfENERfPw" %}
Install .NET 8
{% endembed %}

### SQL Server

Install either the **Developer** or **Express** edition of SQL Server 2022 from the [official](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) page. When following the instructions of the video tutorial, take a good look at the **connection string** property because you are going to use it in your application settings! You are also advised to install **SSMS** as well in order to test your connection string and be able to access harmony's databases.

<figure><img src="../../.gitbook/assets/Screenshot 2024-04-18 154250.png" alt=""><figcaption><p>Connection string</p></figcaption></figure>

{% embed url="https://www.youtube.com/watch?v=FFp5BLoQLAA" %}
SQL Server & SSMS installation
{% endembed %}

#### SQL Server connection strings configuration

First of all ensure that you have the correct connection string. Here are few examples that explain how a connection string looks like:

```json
  "ConnectionStrings": {
    "HarmonyConnection": "Server=.;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  },
```

The above connection string is probably the most used one for local development. **Server=.;** will connect to the [default](https://learn.microsoft.com/en-us/sql/sql-server/connect-to-database-engine?view=sql-server-ver16\&tabs=sqldb#connect-to-a-default-sql-server-instance-on-the-same-machine) SQL Server instance that is installed on your machine and **Integrated Security=True;TrustServerCertificate=True** means Windows authentication will be used.

This is different though in case you have a **named** instance, e.g. SQLEXPRESS _(used in the video)._ In this case use **Server=.\\\<instance name>;** rather than **Server=.;** where SQLEXPRESS is the name of your instance _(yours might be different)._ Example:

```json
  "ConnectionStrings": {
    "HarmonyConnection": "Server=.\SQLEXPRESS;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  },
```

Instead of the dot ("**.**") you can use **localhost** or your PC name as well. For example:

```json
  "ConnectionStrings": {
    "HarmonyConnection": "Server=localhost\SQLEXPRESS;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  },
```

The previous connection strings, use **Integrated Security=True** so the current Windows account credentials are used for authentication _(Windows authentication)._ \
_You can also use **SQL Server Authentication**_ by passing a User Id & Password instead like the following example:

```json
  "ConnectionStrings": {
    "HarmonyConnection": "Server=localhost\SQLEXPRESS;Database=Harmony;User Id=harmony_user;Password=%MySuperPass12345;TrustServerCertificate=True"
  },
```

This login must exist on your SQL Server & have access to harmony databases.\
The previous example, uses an SQL Server login `harmony_user` & password `%MySuperPass12345`instead of **Integrated Security=True.**&#x20;

In case you need to know more about SQL Server connection strings, check [this](https://www.connectionstrings.com/sql-server/) page.

{% hint style="warning" %}
In all the SQL Server connection strings that you are going to configure, only change the **Server** part and optionally replace the **Integrated security** with _User Id & Password_ as described above. The **Database** part must not change!
{% endhint %}

Now that you have your SQL Server connection strings, configure the following 3 properties in the corresponding **appsettings.json** files:

* **Harmony.Api**: The _appsettings.json_ file contains a `ConnectionStrings:HarmonyConnection` property that holds the connection string your SQL Server <mark style="color:orange;">harmony</mark> database.
* **Harmony.Automations**: The _appsettings.json_ file contains a `ConnectionStrings:HarmonyJobsConnection`  property that holds the connection string your SQL Server <mark style="color:orange;">Harmony.Automations.Jobs</mark> database.
* **Harmony.Notifications**: The _appsettings.json_ file contains a `ConnectionStrings:HarmonyJobsConnection`  property that holds the connection string your SQL Server <mark style="color:orange;">Harmony.Notifications.Jobs</mark> database.

### MongoDB

MongoDB is very easy to install. Just navigate to the official [page](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/), download the installer and run it.

{% embed url="https://www.youtube.com/watch?v=gB6WLkSrtJk" %}
Installing MongoDB on windows
{% endembed %}

The default port the MongoDB server listens to is the 27017 and it's already configured for you in the **Harmony.Automations** appsettings.json file.

```json
"MongoDB": {
  "ConnectionURI": "mongodb://localhost:27017"
},
```

Unless you have configured your MongoDB server to run on a different port, you don't have to do anything else.









..to be continued for the rest of the installations
