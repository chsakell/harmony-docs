# 🔥 Technology

Harmony's **codebase** is a state of the art & [_dockerized_](../configuration/docker/) :whale: solution, built with best practices, patterns and a clean microservice architecture. You will be impressed by how easily can be scaled and maintained on a codebase level by adding new features or on infrastructure level by scaling horizontally.&#x20;

{% hint style="success" %}
If you are an engineer or developer, you will be certainly benefit just by studying its codebase.
{% endhint %}

The stack, tools, frameworks used in Harmony are the following:

| Databases                           | Server                                                                                   | Front                                                                    |
| ----------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| SQL Server                          | .NET Core                                                                                | [Blazor](https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor) |
| [MongoDB](https://www.mongodb.com/) | SignalR                                                                                  | [MudBlazor](https://mudblazor.com/)                                      |
| Redis                               | [gRPC](https://learn.microsoft.com/en-us/aspnet/core/grpc/?view=aspnetcore-8.0) :rocket: |                                                                          |

| Data Access      | Patterns           | Messaging                         |
| ---------------- | ------------------ | --------------------------------- |
| Entity Framework | Clean architecture | [RabbitMQ](https://rabbitmq.com/) |
| Dapper           | CQRS MediatR       |                                   |
|                  |                    |                                   |

<figure><img src="../.gitbook/assets/harmony-architecture.gif" alt=""><figcaption><p>Harmony architecture</p></figcaption></figure>

#### Docker containers

Harmony is entirely dockerized with [Kubernetes](../configuration/docker/kubernetes.md) support.

<figure><img src="../.gitbook/assets/harmony-containers.png" alt="" width="563"><figcaption></figcaption></figure>

#### Database diagram

<figure><img src="../.gitbook/assets/database-diagram.png" alt=""><figcaption><p>Harmony's domain entities</p></figcaption></figure>

#### Read next - Configure Harmony

{% content-ref url="../configuration/dependencies/" %}
[dependencies](../configuration/dependencies/)
{% endcontent-ref %}
