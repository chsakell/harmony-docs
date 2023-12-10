---
description: Make sure you read the changelog every time a new update is released on Envato
---

# 🔃 Changelog

Change log contains all version updates, old, current and upcoming with all important additions, fixes or improvements. The **unchecked** items of the _work in progress_ version, are the items remained before this build is released.

### \[Version 1.4] -  Work in progress

{% hint style="info" %}
This version goal is to add the first **charts** for monitoring a project's progress :tada:. \
Also support for **adding comments** in cards will be added :speaking\_head:
{% endhint %}

* [x] Create comment&#x20;
* [x] Edit comment
* [x] Delete comment
* [ ] Implement story points for issues
* [ ] Add charts for scrum boards
* [x] View card in **Fullscreen** :clap: :100:

<figure><img src="../.gitbook/assets/card-comments.gif" alt=""><figcaption><p>Comments &#x26; Fullscreen preview</p></figcaption></figure>

### \[Version 1.3] -  available on [Envato](https://codecanyon.net/item/harmony-project-management-tool/49138488)

{% hint style="info" %}
This version's goal was to **add email notifications** using Gmail configuration and optional email service provider such as Brevo. For best practices and scaling capabilities, a new project was created, responsible to send the notifications. The entire setup will be based on **RabbitMQ** messaging & **HangFire**.

Other than that, there were **fixes** and an amazing UX improvement on the board level and specifically when **moving cards**. It became more responsive while it also syncs the board status across all connected members
{% endhint %}

* [x] **RabbitMQ** messaging integration
* [x] New **Harmony.Notifications** web application with **HangFire** integration for background jobs
* [x] Add **Gmail** integration for email notifications
* [x] &#x20;Add [Brevo](https://www.brevo.com/products/transactional-email/) integration for email notifications

#### UI changes

* [x] Add **due date reminder field** on card's date edit view _(e.g. 1 day before, 1 hour before, etc..)_
* [x] 3 default board lists are added for Kanban projects as well
* [x] Add user options for enabling/disabling notifications
* [x] UX board improvements

#### Notifications implemented so far

* [x] Member added to a workspace
* [x] Workspace access revoked from member
* [x] Member added to a board
* [x] Board access revoked from member
* [x] Issue completed _(moved to a list marked as DONE)_
* [x] Due date based on due date reminder type
* [x] Member assigned to card
* [x] Member has given access to board
* [x] Board access revoked

#### Fixes

* [x] Fixes for card movement in the board, especially with positioning
* [x] Fix typo error when building in **Release** mode
* [x] Ensure _Files_ folder exists inside the published folder when publishing **Harmony.Server** web app

### \[Version 1.2] - 24 November 2023

* [x] Add support for Scrum Projects! :clap: :heart::rocket:
* [x] Create backlog
* [x] Create sprints
* [x] Start a sprint
* [x] Complete a sprint & move any pending items to backlog or a different sprint
* [x] Move items from sprint to backlog
* [x] Move items from backlog to sprints
* [x] Introduce issue types for cards _(Epic, Bug, Task, Story, Task)_
* [x] Create **Serial Key** to easily identify cards/issues, e.g. HARM-1[^1]
* [x] Create board/sprint navigation items _(left menu & breadcrumbs)_
* [x] Change backlog items order

#### Enhancements

* [x] Add remove button to remove attachments
* [x] Clear card's dates
* [x] Update user's profile picture in all user card's components after uploading a picture in account page
* [x] Update text on **Enter** key pressed on _EditableTextField_ component
* [x] Display loader when creating cards
* [x] Scroll to the card's position added when creating a card within a list filled with multiple cards

### \[Version 1] - 14 November 2023

* [x] User registration / login
* [x] Create workspaces
* [x] Add users to workspaces
* [x] Create **KANBAN** boards
* [x] Add users to boards
* [x] Add lists to boards
* [x] Rename & reorder lists
* [x] Add cards to boards
* [x] Move cards
* [x] Assign users to cards
* [x] Add labels to cards
* [x] Toggle labels, add custom labels to board/cards
* [x] Add checklists to cards
* [x] Add check list items to checklists
* [x] Add start / due date to cards
* [x] Register some activities

#### Read next - Technology

{% content-ref url="technology.md" %}
[technology.md](technology.md)
{% endcontent-ref %}

[^1]: First issue for a project named Harmony
