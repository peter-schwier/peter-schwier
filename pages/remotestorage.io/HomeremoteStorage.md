---
title: Home
pageTitle: Home | remoteStorage
length: 3243
author: 
timestamp: 2023-04-16T14:47:31-05:00
markdownload-tags: []
markdownload-source: https://remotestorage.io/
markdownload-hostname: remotestorage.io
---

# Home | remoteStorage

## Excerpt
> Open protocol for per-user storage on the Web

---
![][fig1]

___

## [](https://remotestorage.io/#features)Features

## [](https://remotestorage.io/#for-users)For users

### [](https://remotestorage.io/#own-your-data)Own your data

Everything in one place – your place. Use a storage account with a provider you trust, or set up your own storage server. Move house whenever you want. It’s your data.

### [](https://remotestorage.io/#stay-in-sync)Stay in sync

remoteStorage-enabled apps automatically sync your data across all of your devices, from desktop to tablet to smartphone, and maybe even your TV or VR headset.

### [](https://remotestorage.io/#compatibility--choice)Compatibility & choice

Use the same data across different apps. Create a to-do list in one app, and track the time on your tasks in another one. Say goodbye to app-specific data silos.

### [](https://remotestorage.io/#go-offline)Go offline

Most remoteStorage-enabled apps come with first-class offline support. Use your apps offline on the go, and automatically sync when you’re back online.

## [](https://remotestorage.io/#for-developers)For developers

### [](https://remotestorage.io/#backend-as-a-service)Backend as a Service

Develop your web app without worrying about hosting or even developing the backend for it: your users will connect their own backend at runtime.

### [](https://remotestorage.io/#infinite-scalability)Infinite Scalability

No matter if 5 hundred or 5 million users are using your app, your backend scales automatically and never costs you a single cent.

### [](https://remotestorage.io/#wheels-included)Wheels Included

remoteStorage.js is a JavaScript library that does all the heavy-lifting of connecting to any remoteStorage backend, caching, synchronizing and storing user data.

[Browse apps](https://remotestorage.io/apps) [Get storage](https://remotestorage.io/servers)

___

## [](https://remotestorage.io/#developer-library)Developer library

The [remoteStorage.js](https://github.com/remotestorage/remotestorage.js) library does most of the heavy lifting to add offline storage and cross-device synchronization to your apps. No more worrying about accounts, databases, passwords…

## [](https://remotestorage.io/#setup)Setup

```
const rs = new RemoteStorage();
rs.access.claim('todos', 'rw');
rs.caching.enable();

const client = rs.scope('/todos/');
```

## [](https://remotestorage.io/#write-an-object)Write an object

```
// Declare an object type to validate if you want (JSON Schema)
client.declareType('todo-item', {});

// Write `{"id":"alfa","done":false}` to /todos/alfa.json
await client.storeObject('todo-item', 'alfa.json', {
  id: 'alfa',
  done: false,
});
```

## [](https://remotestorage.io/#get-objects)Get objects

```
const specificItem = await client.getObject('alpha.json');
const allTodoItems = await client.getAll();
```

Use our [drop-in UI widget](https://github.com/remotestorage/remotestorage-widget) for connecting remote storage accounts.

```
const widget = new Widget(rs);
widget.attach();
```

[Read the documentation](https://remotestoragejs.readthedocs.io/) [Protocol details](https://remotestorage.io/protocol)

___

remoteStorage is a grass-roots standard, developed completely in the open, by the community for the community. Countless individuals have contributed in one way or another over time, and we’d love to welcome you as one of them!

<table><tbody><tr><td><a href="https://github.com/remotestorage">GitHub</a></td><td>Where we collaborate on the protocol specification as well as all common source code.</td></tr><tr><td><a href="https://community.remotestorage.io/">Forums</a></td><td>Our community exchange and support site for everybody from users to developers to providers.</td></tr><tr><td><a href="https://web.libera.chat/#remotestorage">IRC</a></td><td>Some community members are hanging out in #remotestorage on Libera.Chat — say hi!</td></tr><tr><td><a href="https://twitter.com/remotestorage_">Twitter</a> / <a href="https://kosmos.social/@remotestorage">Fediverse</a></td><td>Follow the project on Twitter or on the Fediverse, to receive updates on releases, events, apps, and related news.</td></tr><tr><td><a href="https://buttondown.email/remotestorage">Mailing List</a></td><td>A monthly digest about remoteStorage apps, tools, and decentralized news.</td></tr><tr><td><a href="https://community.remotestorage.io/c/events">Events</a></td><td>Meet people in person at conferences, hackathons, camps, and other gatherings.</td></tr></tbody></table>

We would love for you to get involved — check out [What can I do for remoteStorage?](https://remotestorage.io/contribute) for some ideas.

## [](https://remotestorage.io/#thank-you-to-our-contributors)Thank you to our contributors!

… and everyone not listed here!

[Join our forum](https://community.remotestorage.io/) For everybody from users to developers to providers.

___

___

[fig1]: https://remotestorage.io/img/icon.svg

> Saved from https://remotestorage.io/ on 2023-04-16T14:47:31-05:00