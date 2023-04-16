---
title: Actual: Using CRDTs in the Wild
pageTitle: Actual: Using CRDTs in the Wild
length: 10740
author: 
timestamp: 2023-04-16T15:00:18-05:00
markdownload-tags: []
markdownload-source: https://archive.jlongster.com/using-crdts-in-the-wild
markdownload-hostname: archive.jlongster.com
---

# Actual: Using CRDTs in the Wild

## Excerpt
> James Long's blog

---
I'm building [Actual](https://actualbudget.com/), an app for managing your finances. totally offline and local, and changes sync across devices

```
function yes() {}// just do ityes()
```

![Screen Shot 2019-06-05 at 11 03 52 PM][fig1]

-   Launched in January, [#4 product of the day on Product Hunt](https://www.producthunt.com/posts/actual-budget), ~35 paid subscribers, will re-launch in the summer with better branding and more features
    
-   Didn't mean to build it with syncing. I'm a product guy, just trying to make this work
    
-   During development it became clear that a mobile app is of critical importance to personal finances
    
-   A problem: I was banking on keeping this simple and just storing data locally. How to sync…
    
-   Dropbox? Nope… Started hacking on own naive syncing algorithm (described in [this gist](https://gist.github.com/jlongster/6e2af5bb3ae3ab229cafab06efc724fc))
    
-   Thought my requirements (syncing is a "nice to have", only a few clients syncing, changes are relatively far apart time-wise) would make it passable. Turns out, there is no free lunch with anything distributed
    
-   Friends had been nagging me to use CRDTs. Reluctantly looked into them and eventually became convinced - started to refactor app. It wasn't too bad!
    
-   Log changes, applying them in any order (they are commutative), verify data with merkle tree, and boom I had reliable syncing. There were many small consequences to converting to CRDTs which I'll go into later, but I was amazed and starting viewing all apps differently
    
-   Suddenly I had an app that _felt_ online but it was really offline and local! And it actually felt _better_ than other online apps because syncing shows changes immediately, so it feels like it's _real-time_ instead of having to refresh a page all the time.
    
-   Not only does it feel like a better online app, you have all these other advantages over cloud apps:
    
    -   Access it whenever you want, regardless of offline, slow connection, or online
    -   Data always loads fast (immediately), regardless of network
    -   You _really_ own your data, it's literally sitting in a local sqlite file
    -   It's totally private - even if messages are synced through a centralized server, there's no reason they can't be end-to-end encrypted
    -   The app will continue running forever as long as it runs on the OS. There's no server which can die and take your data with you
-   This is the future! local -> cloud -> local & online
    
-   Very quickly realized how half-assed other apps' "offline" functionality was:
    

## Keynote

![sync-conflict-100635256-large970 idge][fig2]

## Trello

![Screen Shot 2019-05-13 at 6 27 07 PM][fig3]

## Notion

![Screen Shot 2019-06-05 at 9 19 49 PM][fig4]

## Airtable

![Screen Shot 2019-06-05 at 9 20 29 PM][fig5]![Screen Shot 2019-06-05 at 9 26 17 PM][fig6]![Screen Shot 2019-06-05 at 9 25 45 PM][fig7]

-   Today, your app is not an "offline" app if it cannot be reliably synced always. And from what I see, the best way to provide that is via CRDTs. Keep your data truly local, and sync changes back and forth when possible. Almost nobody is doing this.
    
-   Demo
    
    -   Start app, start app totally offline
    -   Start mobile app, load budget
    -   Basic syncing
    -   Enable real-time with setInterval (then disable)
    -   Go offline, make some changes, come back online
-   How does it work?
    
    -   App uses a normal sqlite database to read data. Normal tables, can do any kind of sqlite SELECT queries that you want (good performance, works well with financial data)
        
    -   Any changes do through `insert`, `update`, and `delete` functions internally. These functions always work with a single item and generate "messages" to update the data.
        

```
insert("transactions", {  id: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  name: "Kroger",  amount: 450})// becomes{  dataset: "transactions",  row: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  column: "name",  value: "Kroger"}{  dataset: "transactions",  row: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  column: "amount",  value: 450}
```

```
update("transactions", {  id: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  name: "Kroger",  amount: 450})// becomes{  dataset: "transactions",  row: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  column: "name",  value: "Kroger"}{  dataset: "transactions",  row: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  column: "amount",  value: 450}
```

```
delete("transactions", {  id: "30127b2e-f74c-4a19-af65-debfb7a6a55b"})// becomes{  dataset: "transactions",  row: "30127b2e-f74c-4a19-af65-debfb7a6a55b",  column: "tombstone",  value: 1}
```

-   ids are always unique ids, never sequentially increasing integers, to avoid conflicts
-   These messages are given to the syncing layer which applies them and also send them to the server (if online)
-   There is no difference between the local client making updates to the data and receiving changes from another client. It all goes through the same layer
-   How are these messages stored? A special `messages_crdt` table inside the database

```
CREATE TABLE messages_crdt (timestamp TEXT NOT NULL UNIQUE,  dataset TEXT NOT NULL,  row TEXT NOT NULL,  column TEXT NOT NULL,  value BLOB NOT NULL);
```

-   When a message is applied, it both updates the appropriate table (like `transactions`) _and_ is added to the `message_crdt` table. This means all data is stored twice, but the database is so small that the file size increase is worth it (it's not even quite 2x due to sqlite's optimizations). A big database might be only 16MB.
    
-   Syncing involves figuring out which messages to send, sending them to server, and receiving any new messages and applying them
    

```
client A   <—>   server   <—>   client B
```

-   The server is basically a thin client that keeps messages and other data and moves them around the same way the client does
    
-   Wait… what about message ordering?
    
-   The key which simplifies all of this: hybrid logical clocks (HLCs). Paper here: [https://cse.buffalo.edu/tech-reports/2014-04.pdf](https://cse.buffalo.edu/tech-reports/2014-04.pdf). The `timestamp` field of the `messages_crdt` table is a HLC
    
-   A HLC is a globally-unique, monotonic timestamp. looks like this:
    

```
2019-06-03T16:40:53.876Z-0000-9f66d38cba0ef956
```

-   This is a collatable string that can simply be compared string-wise to determine ordering. Made up of 3 parts:

```
2019-06-03T16:40:53.876Z - 0000 -    9f66d38cba0ef956^^^                        ^^^       ^^^ local time                 counter   node id
```

-   Don't have time to explain all of it, but it basically turns time into something you can rely on. Giving all messages this kind of timestamp gives them causal ordering
    
-   This works by assuming clients all relatively have the same clock. A drift time of 1 minute is allowed but if a client's clock differs for more than a minute it's rejected
    
-   The system keeps track of the latest HLC it's ever seen, and will use that time to generate future timestamps. So if it's synced from a client with a clock slightly in the future, it won't always generate timestamps before it.
    
-   The `counter` is key: if multiple messages with the same local time are generated the counter will tick upwards giving them reliable ordering. Ultimately, if two clients pressed a button at exactly the same time, the node id will determine who wins
    
-   The last key to all of this: a merkle tree. A merkle tree is a tree of hashes representing which messages have been applied. You can simply compare the top-level hash of a merkle tree between two clients to see if they are in sync. If they are not, you can traverse down the merkle tree and see how far back in time they are out of sync, whether it's 5 seconds, 30 seconds, 1 minute, 5 minutes, or more. It then queries the server for all messages since this time.
    
-   The merkle tree also provides a way to tell the user if something is wrong. If the hash of the merkle tree cannot ever be matched, the user is notified that they need to reset syncing
    
-   Advantages
    
    -   Fully conflict-free, the user never has to worry about conflicts (even the chance to manually resolve doesn't make sense in an app like this)
    -   Updates are made at the field-level, not at the row-level, unlike many other syncing solutions (pouchdb). Changes to different fields on the same item will sync freely.
        -   This actually enabled a real-world use case: I changed the data format of a field so when the user upgraded to that version the app modified that field in every single transaction in the system, sending off an update message for every single transaction for that field. Would conflict with any other changes if it conflicted at the row-level
    -   Never have to worry about the network, dropped messages, or out-of-order messages! Just get new messages whenever possible and apply
    -   HLCs make it relatively straight-forward to think about resolving conflicts: if user A makes a change later then user B, they will most likely win. Matches what the user would expect
    -   I can still use a normal, real sqlite database without anything special for high performance queries
    -   The server is 300 lines of code, it simply passes messages around (technically isn't needed if I went fully p2p, but centralized for convenience)
    -   Simple enough implementation that I was able to implement it completely myself, without any need for using a 3rd party database. Maybe that's foolish, but given how greenfield this tech is, at the time (started researching 2 years ago) there wasn't anything that fulfilled all my needs. The benefit of being custom is it's streamlined for the app's needs.
-   Disadvantages
    
    -   Going from a simple local app to a distributed one requires a big mental jump in how you structure data. No longer can you simply delete items, or make arbitrary updates across complex data. It wasn't too bad since I'm using simple sqlite tables where everything is just a row of fields (no in-memory complex data structures)
    -   Changing the data schema is incredibly difficult. You have to always assume you'll get a message generated by the old schema, so you need to support both or somehow "upgrade" the message. And then there's the problem of an older client getting a message generated by the newer schema.
    -   Things like generating sort orders become more complex. A naive implementation will eventually converge, sure, but the result is less than ideal (you could eventually have many items with the same sort index in a list leading to errors). Need to manage data in a way that is friendly to syncing
    -   No more delete/update queries across data (`UPDATE transactions SET payee = NULL WHERE amount > 500`). Everything has to be done through the syncing layer which works with individual items only
    -   CRDTs (at least operation-based) generate lots of changes quickly, especially since it works per-field, resulting in more data to move around and process when syncing
    -   Can't make any assumptions about the state of the data. If you have two fields that should both be null or not at the same time, you can't enforce that. When they are both set to null two messages will be generated and one of them could be dropped so another client has inconsistent state, though it will eventually converge
    -   The list of changes is append-only, so it's ever growing. Need an "epoch" date that is shifted every so often that represents a "start" time when all messages before it can be deleted

[![][fig8]](https://archive.jlongster.com/)

James Long is a developer & designer with over a decade of experience building large-scale applications. [Get in touch](mailto:longster@gmail.com)

© James Long 2020

[fig1]: https://user-images.githubusercontent.com/17031/59004054-eafb5800-8807-11e9-9022-ef5415ac7e07.png
[fig2]: https://user-images.githubusercontent.com/17031/59002021-783aae80-8800-11e9-9ff3-4a92784ce0c6.png
[fig3]: https://user-images.githubusercontent.com/17031/59002038-8ab4e800-8800-11e9-8faf-8bb2a6de0dfc.png
[fig4]: https://user-images.githubusercontent.com/17031/59002101-cea7ed00-8800-11e9-8592-8518adeb194f.png
[fig5]: https://user-images.githubusercontent.com/17031/59002127-eda67f00-8800-11e9-9063-240e150b976c.png
[fig6]: https://user-images.githubusercontent.com/17031/59002210-2cd4d000-8801-11e9-8226-49d706b63fa1.png
[fig7]: https://user-images.githubusercontent.com/17031/59002202-29414900-8801-11e9-868a-fbbb3f113390.png
[fig8]: https://archive.jlongster.com/img/logo2.svg

> Saved from https://archive.jlongster.com/using-crdts-in-the-wild on 2023-04-16T15:00:18-05:00