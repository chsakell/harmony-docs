# ⚙ Setup

Harmony is a web application built with **.NET Core** which means it's cross platform and can be deployed anywhere _(Windows, Linux, Mac)_.&#x20;

Its dependencies are:

* An **SQL Server** which can be installed on Windows or [Linux](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-setup?view=sql-server-ver16#supportedplatforms).  Read the [databases.md](databases.md "mention") guide for more information about how to setup the required databases.
* **RabbitMQ** for asynchronously  exchanging messages between <mark style="color:blue;">Harmony</mark> web app and <mark style="color:blue;">Harmony.Notifications</mark>. The latter, is the app responsible for sending the email notifications. Read the [rabbitmq.md](rabbitmq.md "mention") section to setup RabbitMQ.
* Email service provider: Harmony needs you to configure an email service provider so that it can send the email notifications. Currently Gmail & [Brevo](https://www.brevo.com/products/transactional-email/) are supported but it's very easy to add your own provider. Read the  [email-provider.md](email-provider.md "mention") guide for more information about how to setup an email provider.
* Search engine (optional): Harmony provides a powerful search engine functionality that can be configured either via direct database search or using a 1st class indexing service named [Algolia ](https://www.algolia.com/products/ai-search/)_(it's free by the way for 10000 req/month)_.. Read the [search](search-engine.md) section for more details.
* **Harmony.Notifications**: A web application that subscribes to RabbitMQ messages sent by <mark style="color:blue;">Harmony.Server</mark> web app and handles email and search index notifications.

{% content-ref url="databases.md" %}
[databases.md](databases.md)
{% endcontent-ref %}

{% content-ref url="rabbitmq.md" %}
[rabbitmq.md](rabbitmq.md)
{% endcontent-ref %}

{% content-ref url="search-engine.md" %}
[search-engine.md](search-engine.md)
{% endcontent-ref %}

{% content-ref url="email-provider.md" %}
[email-provider.md](email-provider.md)
{% endcontent-ref %}

{% content-ref url="deployment.md" %}
[deployment.md](deployment.md)
{% endcontent-ref %}

#### Make Harmony yours - Buy once, get updates for ever :rocket:

{% content-ref url="../buy-online.md" %}
[buy-online.md](../buy-online.md)
{% endcontent-ref %}
