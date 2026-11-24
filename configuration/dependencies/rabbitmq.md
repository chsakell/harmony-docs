# 📬 RabbitMQ

The best guide for installing RabbitMQ is the official website itself. Please follow the installation guide that fits your environment [here](https://www.rabbitmq.com/download.html).

{% hint style="warning" %}
Remember that RabbitMQ requires that you have also installed [Erlang](https://www.rabbitmq.com/which-erlang.html). Don't worry though, the official website explains everything you need to do
{% endhint %}

### Broker configuration

Set the **BrokerConfiguration** setting in the _**appsettings.json**_ files in all web application projects. Following is an example for local RabbitMQ configuration.

```json
  "BrokerConfiguration": {
    "Host": "localhost",
    "Port": 5672,
    "Username": "harmony",
    "Password": "my_super_password",
    "VirtualHost": "/"
  }
```

Harmony creates the exchanges and queues it needs on startup: the `harmony_notifications` topic exchange, the `notifications.automation` & `notifications.signalr` queues, and the `email_notifications` & `search_index_notifications` queues.

{% hint style="info" %}
**.NET Aspire** runs RabbitMQ as a container (AMQP on host port 5673, management UI on [http://localhost:15673](http://localhost:15673), user `harmony`, volume `harmony-rabbitmq-data`). It sets the `BrokerConfiguration__Host`, `__Port`, `__Username`, `__Password` and `__VirtualHost` environment variables of every service. Publishing and consuming messages is traced end to end: one trace follows a message from the publisher to all its consumers. See [aspire.md](../aspire.md "mention").
{% endhint %}

#### Read next: Configuring the email service provider and a search engine

{% content-ref url="search-engine.md" %}
[search-engine.md](search-engine.md)
{% endcontent-ref %}

{% content-ref url="email-provider.md" %}
[email-provider.md](email-provider.md)
{% endcontent-ref %}

{% content-ref url="../deployment.md" %}
[deployment.md](../deployment.md)
{% endcontent-ref %}
