# 💫 Automations

Starting from v2.0 Harmony supports automations :rocket:. \
The purpose of each automation is to automatically run a task for you, a repetitive operation that you would normally do after certain conditions.&#x20;

The first automation supported is the <mark style="color:blue;">Sync parent and child tasks</mark>. This was just the beginning, more to come in the near future!

{% hint style="warning" %}
Automations are stored in a MongoDB database so make sure you have [setup](../overview/setup/databases/mongodb-server.md) all the required dependencies before trying to configure your first automation.
{% endhint %}

### Sync parent and child tasks

This automation is triggered when all of a card's children are moved to a specific status (board list) and conditionally syncs the parent issue.

Navigate to the <mark style="color:blue;">Settings => Automation</mark> view of your board and select to **Configure** the <mark style="color:orange;">Sync parent and child tasks</mark> automation. A drawer will open where you can configure the following options:

* **Name**: Give a descriptive name for your automation. You can create more than one automations for the same type.
* Description: This is a disabled field, automatically filled for you in order to help you understand how the automation will behave.
* **From status**: The parent issue will be synced only if belongs to any of the selected lists. Leave empty if you wish to always sync.
* **To status**: The parent issue will sync when all children are moved to any of the selected lists. Leave empty and the parent issue will always be synced with children.

{% embed url="https://www.youtube.com/watch?v=2GcHRGpwGic" %}
Sync parent and child tasks automation
{% endembed %}

