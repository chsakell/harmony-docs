# 📬 RabbitMQ

The best guide for installing RabbitMQ is the official website itself. Please follow the installation guide that fits your environment [here](https://www.rabbitmq.com/download.html).

{% hint style="warning" %}
Remember that RabbitMQ requires that you have also installed [Erlang](https://www.rabbitmq.com/which-erlang.html). Don't worry though, the official website explains everything you need to do
{% endhint %}

### Broker configuration

Set the **BrokerConfiguration** setting in the _**appsettings.json**_ file in both the <mark style="color:blue;">**Harmony**</mark> & <mark style="color:blue;">**Harmony.Notifications**</mark> web app. Following is an example for local RabbitMQ configuration.

```json
  "BrokerConfiguration": {
    "Host": "localhost",
    "Port": 5672
  },
```

{% hint style="warning" %}
Normally a RabbitMQ configuration would require a **username** and a **password**. This will be released soon in the next build. In case though you want to use it right now there are 3 simple steps to achieve it:
{% endhint %}

1.  Change the <mark style="color:blue;">BrokerConfiguration</mark> class as follow:

    ```csharp
    public class BrokerConfiguration
    {
        public string Host { get; set; }
        public int Port { get; set; }
        public string Username { get; set; } // added
        public string Password { get; set; } // added
        public string VirtualHost { get; set; } // added
    }
    ```
2.  Change the BrokerConfiguration in the _**appsettings.json**_ files as follow and set your RabbitMQ settings:

    ```json
      "BrokerConfiguration": {
        "Host": "",
        "Port": ,
        "Username": "",
        "Password": "",
        "VirtualHost": ""
      },
    ```

Example configuration for a _harmony_ user created on localhost & virtual host /:

```json
  "BrokerConfiguration": {
    "Host": "localhost",
    "Port": 5672,
    "Username": "harmony",
    "Password": "my_super_password",
    "VirtualHost": "/"
  },
```

3. In the <mark style="color:blue;">NotificationsConsumerHostedService</mark> & the <mark style="color:blue;">RabbitMQNotificationPublisher</mark> classes set the new properties added in the <mark style="color:blue;">ConnectionFactory</mark>.

```csharp
var factory = new ConnectionFactory
{
    HostName = _brokerConfiguration.Host,
    Port = _brokerConfiguration.Port,
    AutomaticRecoveryEnabled = true,
    UserName = string.IsNullOrEmpty(_brokerConfiguration.Username) 
        ? ConnectionFactory.DefaultUser : _brokerConfiguration.Username, // add this
    Password = string.IsNullOrEmpty(_brokerConfiguration.Password) 
        ? ConnectionFactory.DefaultPass : _brokerConfiguration.Password, // add this
    VirtualHost = string.IsNullOrEmpty(_brokerConfiguration.VirtualHost) ?
        ConnectionFactory.DefaultVHost : _brokerConfiguration.VirtualHost,// add this
};
```

#### Read next: Configuring the email service provider

{% content-ref url="email-provider.md" %}
[email-provider.md](email-provider.md)
{% endcontent-ref %}

{% content-ref url="deployment.md" %}
[deployment.md](deployment.md)
{% endcontent-ref %}
