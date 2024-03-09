---
description: Centralize your logs in a log management system
---

# 📝 Distributed logging

Harmony uses [Kibana](https://www.elastic.co/kibana) & [Elasticsearch ](https://www.elastic.co/)for centralized, distributed logging. All microservices send their logs to an elastic instance and Kibana is responsible to search, aggregate, and visualize data.

{% hint style="warning" %}
Search functionality is only available using docker containers.&#x20;

Deploy Elasticsearch and Kibana using either the **docker-compose** or the Kubernetes **kubectl** command.
{% endhint %}

### Configure Kibana for searching

Assuming you have deployed the docker containers either using the <mark style="color:orange;">docker-compose</mark> command or using the <mark style="color:orange;">kubectl</mark> to a Kubernetes cluster, Kibana should be up and running on port **5601**.

1. Navigate to [http://localhost:5601/app/home#/](http://localhost:5601/app/home#/) to open Kibana's interface. If it's the first time you will see a welcome message, just click **Explore on my own**.

<div>

<figure><img src="../../../.gitbook/assets/kibana-welcome.png" alt=""><figcaption><p>Welcome to Elastic</p></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/kibana-home.png" alt=""><figcaption><p>Kibana</p></figcaption></figure>

</div>

2. Open the left sidebar and find the **Kibana index patterns** from the management menu, in order to create an index pattern for harmony. Alternative you can just navigate to [http://localhost:5601/app/management/kibana/indexPatterns](http://localhost:5601/app/management/kibana/indexPatterns). Click **Create index pattern** and fill the following values:

* Name: **harmony-\***
* Timestamp field: **@timestamp**

<figure><img src="../../../.gitbook/assets/kibana-index-patterns.png" alt=""><figcaption><p>Create index pattern</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/kibana-create-index-pattern.png" alt=""><figcaption><p>Index pattern for Harmony</p></figcaption></figure>

3. Your index pattern is good to go. Select **Discover** from the left sidebar and start searching!

<figure><img src="../../../.gitbook/assets/kibana-discover-logs.png" alt=""><figcaption><p>Search with Kibana</p></figcaption></figure>
