# Sync parent and child tasks

This automation is triggered when all of a card's children are moved to a specific status (board list) and conditionally syncs the parent issue.

Navigate to the <mark style="color:blue;">Settings => Automation</mark> view of your board and select to **Configure** the <mark style="color:orange;">Sync parent and child tasks</mark> automation. A drawer will open where you can configure the following options:

* **Name**: Give a descriptive name for your automation. You can create more than one automations for the same type.
* Description: This is a disabled field, automatically filled for you in order to help you understand how the automation will behave.
* **From status**: The parent issue will be synced only if belongs to any of the selected lists. Leave empty if you wish to always sync.
* **To status**: The parent issue will sync when all children are moved to any of the selected lists. Leave empty and the parent issue will always be synced with children.

{% embed url="https://www.youtube.com/watch?v=2GcHRGpwGic" %}
Sync parent and child tasks automation
{% endembed %}
