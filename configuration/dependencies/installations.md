---
description: Install and configure all the required dependencies for your local environment
---

# 📀 Installations

As you can see in the architecture diagram, Harmony has a few dependencies that need to be installed and configured before running. These are the followings:

* .NET 8
* SQL Server _(optional)_
* PostgreSQL _(optional)_
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

{% hint style="info" %}
In case you want to use **PostgreSQL** instead of SQL Server, skip this installation and proceed with the PostgreSQL installation.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2024-04-18 154250.png" alt=""><figcaption><p>Connection string</p></figcaption></figure>

{% embed url="https://www.youtube.com/watch?v=FFp5BLoQLAA" %}
SQL Server & SSMS installation
{% endembed %}

#### SQL Server connection strings configuration

First of all ensure that you have the correct connection string. Here are few examples that explain how a connection string looks like:

```json
  "DatabaseProvider": "SqlServer",
  "ConnectionStrings": {
    "HarmonyConnection": "Server=.;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  },
```

The above connection string is probably the most used one for local development. **Server=.;** will connect to the [default](https://learn.microsoft.com/en-us/sql/sql-server/connect-to-database-engine?view=sql-server-ver16\&tabs=sqldb#connect-to-a-default-sql-server-instance-on-the-same-machine) SQL Server instance that is installed on your machine and **Integrated Security=True;TrustServerCertificate=True** means Windows authentication will be used.

This is different though in case you have a **named** instance, e.g. SQLEXPRESS _(used in the video)._ In this case use **Server=.\\\<instance name>;** rather than **Server=.;** where SQLEXPRESS is the name of your instance _(yours might be different)._ Example:

```json
  "DatabaseProvider": "SqlServer",
  "ConnectionStrings": {
    "HarmonyConnection": "Server=.\SQLEXPRESS;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  },
```

Instead of the dot ("**.**") you can use **localhost** or your PC name as well. For example:

```json
  "DatabaseProvider": "SqlServer",
  "ConnectionStrings": {
    "HarmonyConnection": "Server=localhost\SQLEXPRESS;Database=Harmony;Integrated Security=True;TrustServerCertificate=True"
  },
```

The previous connection strings, use **Integrated Security=True** so the current Windows account credentials are used for authentication _(Windows authentication)._ \
_You can also use **SQL Server Authentication**_ by passing a User Id & Password instead like the following example:

```json
  "DatabaseProvider": "SqlServer",
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

### PostgreSQL

Installing PostgreSQL is straight forward. Follow the instructions on the following YouTube video to install it on Windows. Follow a respective guide for Linux installation. After installing, configure the required connection strings / migrations as described on the [#postgresql](installations.md#postgresql "mention") guide.

{% embed url="https://www.youtube.com/watch?v=uN0AfifH1TA" %}
Installing PostgreSQL on Windows
{% endembed %}

### MongoDB

MongoDB is very easy to install. Just navigate to the official [page](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/), download the installer and run it.

{% embed url="https://www.youtube.com/watch?v=gB6WLkSrtJk" %}
Installing MongoDB on windows
{% endembed %}

The default port the MongoDB server listens to is the **27017** and it's already configured for you in the **Harmony.Automations** & **Harmony.Integrations.SourceControl** appsettings.json files.

```json
"MongoDB": {
  "ConnectionURI": "mongodb://localhost:27017"
},
```

Unless you have configured your MongoDB server to run on a different port, you don't have to do anything else.

{% hint style="info" %}
You can use [MongoDB Compass](https://www.mongodb.com/products/tools/compass) to ensure you have installed MongoDB properly.
{% endhint %}

### RabbitMQ

[RabbitMQ](https://www.rabbitmq.com/) is on of the most important components in Harmony's architecture as it's responsible for asynchronously exchanging messages between applications. There are two steps for installing RabbitMQ, first install the [Erlang](https://www.erlang.org/) dependency & then the actual RabbitMQ. Follow the instructions of the YouTube video to install RabbitMQ on your machine.

{% embed url="https://www.youtube.com/watch?v=KhYiaEOrw7Q" %}
Installing RabbitMQ
{% endembed %}

{% hint style="info" %}
Notice that in the video also the management plugin is installed, which let's you see all the exchanges, queues & messages on your RabbitMQ server. It is recommended to enabled it as well.
{% endhint %}

By default RabbitMQ listens to **5672** port which is already been configured for you in all required apps, so unless you have configured the server to listen on a different port, once again you don't have to change anything. The configuration is defined in the following services in their corresponding **appsetting.json** files:

* **Harmony.Api**
* **Harmony.Automations**
* **Harmony.Notifications**
* **Harmony.SignalR**
* **Harmony.Integrations.SourceControl**

```json
"BrokerConfiguration": {
  "Host": "localhost",
  "Port": 5672,
  "Username": "",
  "Password": "",
  "VirtualHost": ""
},
```

{% hint style="info" %}
On production environments, you usually create logins with custom username & password but for your local installation you can leave it as it is.
{% endhint %}

### Redis

Redis installation is completely optional and required only in production environment and only when you have more than one **Harmony.SignalR** instances.

In you want to install and configure it on your local machine, the easiest way is to do it is via a GitHub package which you can find [here](https://github.com/tporadowski/redis/releases).

{% embed url="https://www.youtube.com/watch?v=DLKzd3bvgt8" %}
Installing Redis on Windows
{% endembed %}

Redis is being used on on <mark style="color:blue;">Harmony.SignalR</mark> project so there's only one place to configure it in case you want to use it. Check the **RedisConnectionString** property, inside its **appsettings.json** file.

```json
"RedisConnectionString": ""
```

By default it's an empty string, which means Redis will not be used. In case you install Redis, by default it runs on port **6379** so the connection for your local environment would look like this:

```json
"RedisConnectionString": "127.0.0.1:6379",
```

### Notes

{% hint style="warning" %}
* In the future, all connection strings might be included inside the "**ConnectionStrings**" property so always keep an eye on the [changelog.md](../../overview/changelog.md "mention")when new builds are released.
* Lots of efforts have been made so far and will continue to do so, to keep docs up to date, either for describing the tool's features _(e.g. gifs, YouTube videos)_ or explaining the architecture & how to guides.&#x20;
* <mark style="background-color:red;">**You are responsible**</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">to install & configure Harmony regardless the environment.</mark>&#x20;
* Harmony is an amazing project in many aspects and will keep growing at a fast pace. It's not a simple web app that you can click **F5** and run it. \
  It requires some basic understanding of installing software like all these you read on this page and configuring their connection strings.
* Production, [Docker](../docker/) & [Kubernetes](../docker/kubernetes.md) is a different story. It requires that you understand their concepts and you have experience of deploying solutions based on a microservice architecture.
* You have a lot to learn from a project like this, either by studding the codebase as a software engineer/architect or using it as a project management tool for your projects. Enjoy! :rocket:
{% endhint %}
