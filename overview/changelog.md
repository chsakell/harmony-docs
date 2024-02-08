---
description: Make sure you read the changelog every time a new update is released on Envato
---

# 🔃 Changelog

Change log contains all version updates, old, current and upcoming with all important additions, fixes or improvements. The **unchecked** items of the _work in progress_ version, are the items remained before this build is released.

### \[Version 2.3] - In progress _(instantly available with_ [_GitHub_](pricing.md#github-sponsors) _plan)_

{% hint style="info" %}
After working on architectural improvements for a few sprints in a row, it's high time to get back on track and start adding new features :tada:
{% endhint %}

* [x] Display <mark style="color:red;">archived</mark> cards.
* [x] <mark style="color:green;">Re-activate</mark> archived cards by moving them either to a board (Kanban) or to a sprint (Scrum).
* [x] Sync board across all connections upon card creation or archiving.
* [x] Implement **Smart auto-assign** automation.
* [x] Rename **Harmony.Notifications** HangFire database to **Harmony.Jobs** and re-use it in <mark style="color:blue;">Harmony.Automations</mark> _(breaking - check databases & SQL Server sections)_.

### \[Version 2.2] - 26 January 2024 <a href="#version-2.2" id="version-2.2"></a>

{% hint style="info" %}
This version's goal is to integrate [gRPC](https://learn.microsoft.com/en-us/aspnet/core/grpc/?view=aspnetcore-8.0) communication between internal microservices. This will finalize the solution design towards a **dockerized** microservice architecture.

* The only application that has direct access to the main harmony database is the <mark style="color:blue;">Harmony.Api</mark>.
* The only application that has direct access to the MongoDB automations database is the <mark style="color:blue;">Harmony.Automations</mark>.
* Any interaction between backend microservices should be done through high performant communication using **gRPC** framework :rocket:

\
_**gRPC** is a modern open source high performance Remote Procedure Call (RPC) framework that can run in any environment. It can efficiently connect services in and across data centers with pluggable support for load balancing, tracing, health checking and authentication._
{% endhint %}

* [x] Expose gRPC services from Harmony.Api.
* [x] Expose gRPC service from Harmony.Automations.
* [x] Remove direct harmony database access from <mark style="color:blue;">Harmony.Automations</mark> and consume gRPC service from Harmony.Api instead.
* [x] Remove direct harmony database access from <mark style="color:blue;">Harmony.Notifications</mark> and consume gRPC service from Harmony.Api instead.
* [x] Remove direct MongoDB database access from <mark style="color:blue;">Harmony.Api</mark> and consume gRPC service from Harmony.Automations instead.

### \[Version 2.1]  - 23 January 2024 <a href="#version-2.1" id="version-2.1"></a>

{% hint style="info" %}
This version will upgrade the entire solution to .NET 8.0 :rocket: :tada:
{% endhint %}

* [x] Upgrade solution projects to .NET 8.0
* [x] Convert <mark style="color:blue;">Harmony.Client</mark> to **Standalone Blazor WebAssembly** project.
* [x] Rename <mark style="color:blue;">Harmony.Server</mark> to **Harmony.API** _(microservice)_.
* [x] Rename Harmony.Server.SignalR to **Harmony.SignalR**.
* [x] Remove all MongoDB database dependencies/access from all projects except from Harmony.Automations _(Harmony.Automations will be a microservice)_.
* [x] Expose HTTP endpoints for internal microservice communication.

### \[Version 2.0] - 20 January 2024 <a href="#version-2.0" id="version-2.0"></a>

> The first rule of any technology used in a business is that **automation** applied to an efficient operation will magnify the efficiency. The second is that **automation** applied to an inefficient operation will magnify the inefficiency.
>
> [_**Bill Gates**_](#user-content-fn-1)[^1]

{% hint style="info" %}
This version will bring 3 major upgrades which will level up Harmony even more:

* Add the very first **automation**. The first automation template that will be implemented is the _"<mark style="color:orange;">Auto close parent issue when all sub-tasks are done</mark>"._
* Create a scalable **SignalR** web app responsible for pushing the board related updates to clients.  Another scaling issue to be solved.
{% endhint %}

Version 2.0 will set things up towards a [**dockerized**](https://www.docker.com/) solution. Sit back and relax, this might take a while.

* [x] Add support for child issues to cards.
* [x] Change issue status from card's view.
* [x] Display total number of child issues per card on board.
* [x] Create [**Harmony.Automations**](../guide/automations.md) project for handling automations.
* [x] Create Automations settings page.
* [x] Create, enable, disable and remove an automation.
* [x] Create [**Harmony.Server.SignalR**](setup/databases/redis.md) project for handling SignalR messages.

### \[Version 1.5] - 2 January 2024 <a href="#version-1.5" id="version-1.5"></a>

> _We are searching for some kind of **harmony** between two intangibles: a form which we have not yet designed and a context which we cannot properly describe._\
> \
> [_**Christopher Alexander**_](#user-content-fn-2)[^2]

{% hint style="info" %}
This version's goal is to add a **powerful Search textbox** at the top bar :mag\_right: .  \
Here are some of the features to be implemented in this release:
{% endhint %}

#### Features

* [x] Integrate [Algolia](https://www.algolia.com/) search engine for super fast indexing & searching capabilities. Algolia is totally [free](https://www.algolia.com/pricing/) to use for **10000** requests per month  :muscle: and probably the easiest search engine to configure, existing at the moment
* [x] Implement a database fallback search service in case there's no search engine integration configuration
* [x] Add app bar search textbox for fast results
* [x] Create search modal option for advanced searching scenarios _(based on several criteria, e.g. issue status,  members assigned, board etc.)_&#x20;
* [x] Add **caching** to database operations

For more info on how to setup the search engine read the [search-engine.md](setup/search-engine.md "mention") docs.

{% hint style="info" %}
**Why Algolia & not Elastic Search?**\


* Algolia search is [faster](https://www.algolia.com/blog/engineering/full-text-search-in-your-database-algolia-versus-elasticsearch/), more relevant, and easier to use than Elasticsearch for searching simple database data. Algolia's focus is on getting exceptional results with minimal configuration.&#x20;
* Elasticsearch is more flexible for searching complex data and can be used for a wider range of search applications.

\
<mark style="color:blue;">Who said Harmony won't add Elastic Search support in the future?</mark> :sunglasses:\ <mark style="color:blue;">Harmony is here to stay.</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**Elastic Search**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">support will be added as well.</mark>
{% endhint %}

### \[Version 1.4]  -  18 December 2023

{% hint style="info" %}
This version's goal is to add the first **charts** for monitoring a scrum project's progress :tada:. \
Also support for **adding comments** in cards will be added :speaking\_head:
{% endhint %}

#### Features

* [x] Create, edit, delete comments :writing\_hand::pencil:
* [x] Implement **story points** for issues
* [x] Add burndown chart report for scrum sprints :chart\_with\_upwards\_trend:
* [x] View card in **Fullscreen** :clap: :100:
* [x] Display number of comments on board
* [x] Update backlog item inline :muscle::clap:
* [x] Update sprint item inline :tada:

#### Fixes

* [x] Fix RabbitMQ configuration _(add username, password, virtual host)_
* [x] Fix validation issue on edit sprint modal
* [x] Load board query fixes for scrum projects

<figure><img src="../.gitbook/assets/burndown-report-dark.png" alt=""><figcaption></figcaption></figure>

<div>

<figure><img src="../.gitbook/assets/card-comments.gif" alt=""><figcaption><p>Comments &#x26; Fullscreen preview</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/burndown-report-light.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/issues-overview-light.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/issues-overview-dark.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/sprint-table.png" alt=""><figcaption></figcaption></figure>

</div>

### \[Version 1.3] - 6 December 2023

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
* [x] Create **Serial Key** to easily identify cards/issues, e.g. HARM-1[^3]
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

[^1]: William Henry Gates III (born October 28, 1955) is an American businessman, investor, philanthropist, and writer best known for co-founding the software giant [Microsoft](https://en.wikipedia.org/wiki/Microsoft)

[^2]: Christopher Wolfgang John Alexander (4 October 1936 – 17 March 2022) was an Austrian-born British-American architect and design theorist. He was an emeritus professor at the University of California, Berkeley. His theories about the nature of human-centered design have affected fields beyond architecture, including urban design, software, and sociology.

[^3]: First issue for a project named Harmony
