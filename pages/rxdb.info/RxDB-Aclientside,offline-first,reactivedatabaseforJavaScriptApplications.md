---
title: RxDB - A client side, offline-first, reactive database for JavaScript Applications
pageTitle: RxDB - A client side, offline-first, reactive database for JavaScript Applications
length: 1686
author: 
timestamp: 2023-04-16T15:03:57-05:00
markdownload-tags: []
markdownload-source: https://rxdb.info/
markdownload-hostname: rxdb.info
---

# RxDB - A client side, offline-first, reactive database for JavaScript Applications

## Excerpt
> RxDB (short for Reactive Database) is a NoSQL-database for JavaScript Applications like Websites, hybrid Apps, Electron-Apps, Progressive Web Apps and Node.js.

---
## Realtime applications **made easy**

From the results of a query, to a single field of a document, with RxDB you can **observe everything**. This enables you to build realtime applications **fast** and **reliable**. Whenever your data changes, your UI reflects the new state.

Write await collection.upsert({  
  id: 'foobar',  
  color: '#8d2089'  
});

Observe await collection.findOne('foobar')  
 .$ // get observable  
 .subscribe(d \=> {  
   screen.backgroundColor \= d.color;  
 });

![RxDB][fig1]

![RxDB][fig2]

[fig1]: https://rxdb.info/files/logo/logo.svg
[fig2]: https://rxdb.info/files/logo/logo.svg

> Saved from https://rxdb.info/ on 2023-04-16T15:03:57-05:00