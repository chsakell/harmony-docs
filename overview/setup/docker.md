---
description: Accelerate how you build, test and deploy Harmony
---

# 🐳 Docker

Harmony has adapted a clean, microservice architecture and [**docker** ](https://www.docker.com/)sits on top of this to make building and deploying the entire infrastructure with just a few commands.&#x20;

<figure><img src="../../.gitbook/assets/harmony-containers.png" alt="" width="563"><figcaption><p>Harmony containers</p></figcaption></figure>

The following instructions will guide you to build Harmony's images and run its containers on your local machine.

#### Create and trust a development certificate

Open a PowerShell session and run the following commands to create and trust a development certificate, since all Harmony applications run under HTTPS.

```powershell
dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\harmony.pfx -p HarmonyTeamsSecretKey
dotnet dev-certs https --trust
```

The above commands are for **Windows using Linux** containers. If you are on a **Mac** or **Linux** you need to run:

```powershell
dotnet dev-certs https -ep ${HOME}/.aspnet/https/harmony.pfx -p HarmonyTeamsSecretKey
dotnet dev-certs https --trust
```

Please read the official Microsoft's [guide](https://learn.microsoft.com/en-us/aspnet/core/security/docker-compose-https?view=aspnetcore-8.0) for more details in case you face any problems. The previous commands used a password _HarmonyTeamsSecretKey_ but you can use your own. If you do so make sure you change it on the **docker-compose.yml** file as well.

#### Build docker images and run the containers

Navigate at the root of Harmony's solution folder where the _**docker-compose.yml**_ file exists and run the following commands:

```docker
docker compose build
docker compose up
```

{% hint style="info" %}
In case you are running Harmony via Visual Studio, make sure to stop it before running the `docker compose up` command because the same ports will be exposed by the docker containers. Also if you have MongoDB or/and RabbitMQ servers running on your local machine, you may want to stop them so they don't interfere with the ports exposed by the corresponding containers. \
\
If you don't want to stop them, you can remove the exposed ports from the **docker-compose.yml**. The ports have been exposed for your own convenience, for example if you want to access the container's RabbitMQ management UI from localhost, but they are not required.

```docker
   rabbitmq:
    image: rabbitmq:3-management-alpine
    container_name: harmony_rabbitmq
    ports:
        - 5672:5672
        - 15672:15672
```
{% endhint %}

#### Open Harmony

Assuming you have build and run all the containers properly, you can navigate at [http://localhost:7096/](http://localhost:7096/) and confirm that the page loads smoothly.&#x20;

{% hint style="info" %}
Currently the standalone Blazor WASM project <mark style="color:blue;">Harmony.Client</mark> is served under **Nginx** via HTTP but it will change soon to HTTPS as well.
{% endhint %}

Of course, there will be more changes, improvements and additions for dockerizing Harmony in the future, e.g Kubernetes support, production overrides, etc..
