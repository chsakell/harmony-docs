# 🏃♂ Before running

### Running the apps through Visual Studio

In case you want to run or debug both <mark style="color:blue;">**Harmony**</mark> & <mark style="color:blue;">**Harmony.Notifications**</mark> applications from Visual Studio, set the projects as the startup projects by right clicking the solution and selecting **Configure Startup Projects..**

<figure><img src="../../.gitbook/assets/startup-projects.png" alt=""><figcaption></figcaption></figure>

Before starting Harmony, make sure you have completed all the following required steps:

### Required steps before running Harmony

* [x] You have an SQL Server instance and you have configured **HarmonyConnection** & **HarmonyNotificationsConnection** connection strings in all required web applications in their appsettings.json files. Migrations can be run either manually or let the projects create the databases on start.
* [x] You have a **MongoDB Server** instance running and you have configured the MongoDB:ConnectionUI property in the **appsettings.json** for Harmony.Server & Harmony.Automations projects.
* [x] You have a RabbitMQ instance running and you have configured the **BrokerConfiguration** setting in all required projects in their **appsettings.json** file.
* [x] You have a Redis instance up and running and you have configured the **RedisConnectionString** property in the Harmony.Server.SignalR project's appsettings.json file.&#x20;
* [x] **Important!** You have set the **signalrHostUrl** property in the <mark style="color:blue;">**Harmony.Client**</mark> project's **www** folder, in the appsettings.json file, equal to Harmony.Server.SignalR host URL
