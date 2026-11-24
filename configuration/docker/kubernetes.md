---
description: Automate deployment, scaling, and management of Harmony
---

# ☸️ Kubernetes

Harmony can be deployed on a [Kubernetes](https://kubernetes.io/) cluster with a single command. Kubernetes _(k8s)_ lets you automate deploying, scaling and auto-healing your services, and much more.&#x20;

The manifests in the source code are tested on a **local** Kubernetes cluster, ideally created with the [Docker Desktop](https://www.docker.com/products/docker-desktop/) app. You can test scaling deployment replicas or auto-healing by running <mark style="color:orange;">kubectl</mark> commands on your machine.

For production deployment, feel free to make any modifications to match your environment.

Here's the prerequisites to deploy Harmony in a local Kubernetes cluster:

* [x] You have installed and enabled **Kubernetes** on your local machine (Docker Desktop: _Settings > Kubernetes > Enable Kubernetes_).
* [x] You have the [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) tool installed. It includes kustomize (`kubectl apply -k`).
* [x] You have configured a development certificate as described in the [docker](./ "mention") section.

### Manifests layout

The **.k8s** folder uses [kustomize](https://kustomize.io/): a **base** with everything except the relational database, and one **overlay** per database provider.

```
.k8s/
  base/                 namespace "harmony", ConfigMap harmony-common, Secret harmony-secrets
    infra/              MongoDB, RabbitMQ, Redis, Elasticsearch (StatefulSets + PVCs), Kibana, Aspire dashboard
    apps/               api, automations, notifications, signalr, sourcecontrol, api-gateway, client
    secrets.env         development credentials used to generate the harmony-secrets Secret
  overlays/
    sqlserver/          SQL Server StatefulSet + database settings for api, automations, notifications
    postgres/           PostgreSQL StatefulSet + database settings for api, automations, notifications
```

### Create Harmony images

Open a terminal at the root of Harmony's solution where the _docker-compose.yml_ file exists and run the following command to create the docker images. docker compose tags them `harmony-<service>:latest`, which is what the manifests use.

```docker
docker compose build
```

### Create the namespace and the certificate config map

When you configured the development certificate, a **harmony.pfx** file was created in the <mark style="color:orange;">$env:USERPROFILE\\.aspnet\https\\</mark> folder on your machine. Make it available to the cluster with a [config map](https://kubernetes.io/docs/concepts/configuration/configmap/) in the `harmony` namespace:

```powershell
kubectl create namespace harmony
kubectl create configmap cert-config-map -n harmony --from-file $env:USERPROFILE\.aspnet\https\harmony.pfx
```

### Deploy Harmony

From the root of the solution, apply **one** of the overlays:

```powershell
# SQL Server
kubectl apply -k .k8s/overlays/sqlserver

# or PostgreSQL
kubectl apply -k .k8s/overlays/postgres

# watch the pods become ready
kubectl get pods -n harmony -w
```

| | URL |
| --- | --- |
| Harmony | [http://localhost:7096/](http://localhost:7096/) |
| Aspire dashboard (traces, metrics, structured logs) | [http://localhost:18888](http://localhost:18888) |
| Kibana | [http://localhost:5601](http://localhost:5601) |

{% hint style="info" %}
The older instructions used `k create -f .\.k8s\`, where `k` was an alias for `kubectl`. The folder is now a kustomize structure, so use `kubectl apply -k` with an overlay as shown above.
{% endhint %}

### Persistence

SQL Server, PostgreSQL, MongoDB, RabbitMQ, Redis and Elasticsearch run as **StatefulSets** with persistent volume claims (`data-<name>-0`) on the default `hostpath` storage class of Docker Desktop. Deleting or restarting pods keeps the data. `kubectl delete -k .k8s/overlays/sqlserver` keeps the PVCs too. To start from scratch:

```powershell
kubectl delete pvc -n harmony --all
```

### Secrets

Credentials are stored in the `harmony-secrets` Secret, generated from **.k8s/base/secrets.env**, and are injected into the pods with `secretKeyRef`. There are no plain-text passwords in the manifests.

{% hint style="warning" %}
**.k8s/base/secrets.env** contains **development** credentials only. For any shared environment, change the values, or create the `harmony-secrets` Secret yourself with the same keys and remove the `secretGenerator` from **base/kustomization.yaml**. Database passwords are written into the volumes on first start.
{% endhint %}

{% embed url="https://youtu.be/kWQrQri5Dxs" %}
Harmony on Kubernetes
{% endembed %}
