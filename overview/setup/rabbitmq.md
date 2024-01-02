# 📬 RabbitMQ

The best guide for installing RabbitMQ is the official website itself. Please follow the installation guide that fits your environment [here](https://www.rabbitmq.com/download.html).

{% hint style="warning" %}
Remember that RabbitMQ requires that you have also installed [Erlang](https://www.rabbitmq.com/which-erlang.html). Don't worry though, the official website explains everything you need to do
{% endhint %}

### Broker configuration

Set the **BrokerConfiguration** setting in the _**appsettings.json**_ file in both the <mark style="color:blue;">**Harmony.Server**</mark> & <mark style="color:blue;">**Harmony.Notifications**</mark> web app. Following is an example for local RabbitMQ configuration.

```json
  "BrokerConfiguration": {
    "Host": "localhost",
    "Port": 5672,
    "Username": "harmony",
    "Password": "my_super_password",
    "VirtualHost": "/"
  }
```

{% hint style="info" %}
Harmony's **Email & Search Index** notifications handled by the <mark style="color:blue;">Harmony.Notifications</mark> projects have a dependency on the RabbitMQ so make sure you have configure it before using them
{% endhint %}

#### Read next: Configuring the email service provider and a search engine

{% content-ref url="search-engine.md" %}
[search-engine.md](search-engine.md)
{% endcontent-ref %}

{% content-ref url="email-provider.md" %}
[email-provider.md](email-provider.md)
{% endcontent-ref %}

{% content-ref url="deployment.md" %}
[deployment.md](deployment.md)
{% endcontent-ref %}
