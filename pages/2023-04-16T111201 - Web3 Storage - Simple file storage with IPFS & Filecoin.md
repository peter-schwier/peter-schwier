---
markdownload-timestamp: 2023-04-16T11:12:01 (UTC -05:00)
markdownload-tags: []
markdownload-source: https://web3.storage/
markdownload-hostname: web3.storage
markdownload-author: 
---

# Web3 Storage - Simple file storage with IPFS & Filecoin

> ## Excerpt
> With web3.storage you get all the benefits of decentralized storage and content addressing with the frictionless experience you expect in a modern storage solution. It’s fast and it's open.

---
![][fig1]

![][fig2]

## Connecting services with the data layer

Eliminate silos with the web3.storage platform. Using IPFS and other decentralized protocols, create a true data layer that connects you, your users, and the Web, regardless of where content is stored - client-side, in the cloud, or elsewhere.

Sounds hard? It isn’t. Our client libraries are super easy-to-use, abstracting the complexity of these decentralized protocols. And we provide services like data storage designed to natively support these protocols, so things just work without you ever being locked-in.

![][fig3]

```
1  // retrieve.mjs2  3  import { Web3Storage, getFilesFromPath } from 'web3.storage'4  5  const token = process.env.API_TOKEN6  const client = new Web3Storage({ token })7  8  async function retrieveFiles () {9    const cid =10      'bafybeidd2gyhagleh47qeg77xqndy2qy3yzn4vkxmk775bg2t5lpuy7pcu'11 12   const res = await client.get(cid)13   const files = await res.files()14 15   for (const file of files) {16     console.log(`${file.cid}: ${file.name} (${file.size} bytes)`)17   }18 }19 20 retrieveFiles()|
```

![gradient-background][fig4]

![][fig5]

## web3.storage is built for scale

![][fig6]

Users

Join a global community  
storing data on web3

Stored Objects

Store data with our easy  
to use API or JS library

Filecoin Storage Providers

Data is stored trustlessly across a growing network of storage providers via Filecoin

With web3.storage you get all the benefits of decentralized storage and other cutting-edge protocols with the frictionless experience you expect in a modern dev workflow. Check out our docs pages to learn more.

### Open

All data is accessible via IPFS and backed by Filecoin storage, with service authentication using decentralized identity. Create user-centric applications, run verifiable workloads on data, and more - no servers needed, no lock-in, no trust necessary.

### Reliable

We take the best of web2 and web3 to provide infra you can rely on to scale with you. Frustration with decentralized storage is a thing of the past.

### Simple

Start storing in minutes using our simple client library to see how decentralized protocols can work together to unlock your data layer.

![][fig7]

## Frequently Asked Questions

What is meant by the “web3.storage platform”?

web3.storage is a suite of APIs and services that make it easy for developers and other users to interact with data in a way that is not tied to where the data is actually physically stored. It natively uses decentralized data and identity protocols like [IPFS](https://ipfs.tech/), [Filecoin](https://filecoin.io/), and [UCAN](https://ucan.xyz/) that enable verifiable, data- and user-centric application architectures and workflows.

At the core of the platform includes a hosted storage service which can be used to upload and persist data to make it continuously available. The platform also contains additional services like [w3link](https://web3.storage/products/w3link/) and [w3name](https://web3.storage/products/w3name/) that make it easier to create seamless, delightful web experiences utilizing web3 protocols.

![][fig8]

FAQ

What advantages does web3.storage have over traditional hosted storage services?

Because web3.storage uses decentralized data and identity protocols like [IPFS](https://ipfs.tech/) and [UCAN](https://ucan.xyz/), data and identity are referenced in an open way. Data is referenced using [IPFS content identifiers](https://docs.ipfs.tech/concepts/content-addressing/) that are unique to the data, making your data...

What advantages does web3.storage have over other IPFS hosted services?

web3.storage runs on [Elastic IPFS](https://github.com/elastic-ipfs/elastic-ipfs), an open-source, cloud-native, highly scalable implementation of [IPFS](https://ipfs.tech/). We wrote it as the solution to address increasing adoption of web3.storage, which previously used kubo...

![][fig9]

## Get Started with web3.storage

Choose your own way to store and retrieve using web3.storage. Use your email address or GitHub to get 5 GiB storage for free, with paid plans starting at only $3.

![][fig10]

![][fig11]

JS Client Library

Import the lightweight web3.storage library into your project, and enjoy a simple and familiar way to store and retrieve.

HTTP API

`curl -X POST —data-binary “@foo.gif” https://api.web3.storage/upload`

Test or build your project on any stack, using our easy-to-use HTTP API. web3.storage is as flexible as you need it to be.

Web App

Upload your files directly through our Web UI to debug and validate web3.storage’s use case for your project.

## Trusted by the future

web3.storage is the easiest way to enable the decentralized web for anyone, from cutting-edge user-first apps, to traditional products at scale, to hackathon projects.

See what people building the future of the web today have to say, and get started.

![][fig12]

Ryan W.

Capetown, South Africa

I work pretty much exclusively on Web3 applications, and I'm really impressed with web3.storage. It's almost too easy - I didn't run into any stumbling blocks and had a basic implementation of my project in 30 minutes.

## Open product. Open book.

## Open product. Open book.

[fig1]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvY29pbC43NWZiNmZiNi5wbmcuIFRyeSBlaXRoZXIgYW4gYWJzb2x1dGUgb3IgYSByZWxhdGl2ZSB3aXRoIGEgdmFsaWQgcmVmZXJlciBVUkwuIEZvcm1hdDogaHR0cHM6Ly9pbWFnZXMud2ViMy5zdG9yYWdlLzxPUFRJT05TPi88U09VUkNFLUlNQUdFPi4ifX0=
[fig2]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvY3Jvc3MuOTRmOGZmOTAucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig3]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvaGVsaXguMjgzYTM5ZTkucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig4]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvaG9sb2dyYXBoaWMtYmFja2dyb3VuZC5lMmIzNjNkZi5wbmcuIFRyeSBlaXRoZXIgYW4gYWJzb2x1dGUgb3IgYSByZWxhdGl2ZSB3aXRoIGEgdmFsaWQgcmVmZXJlciBVUkwuIEZvcm1hdDogaHR0cHM6Ly9pbWFnZXMud2ViMy5zdG9yYWdlLzxPUFRJT05TPi88U09VUkNFLUlNQUdFPi4ifX0=
[fig5]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvY3Jvc3MuOTRmOGZmOTAucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig6]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvdHJpYW5nbGUuOGEwOTlhMTUucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig7]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvc3F1aWdnbGUuM2M1NWIzMWQucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig8]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBpbWFnZXMvaW5kZXgvY2x1c3Rlci0xLnBuZy4gVHJ5IGVpdGhlciBhbiBhYnNvbHV0ZSBvciBhIHJlbGF0aXZlIHdpdGggYSB2YWxpZCByZWZlcmVyIFVSTC4gRm9ybWF0OiBodHRwczovL2ltYWdlcy53ZWIzLnN0b3JhZ2UvPE9QVElPTlM+LzxTT1VSQ0UtSU1BR0U+LiJ9fQ==
[fig9]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBfbmV4dC9zdGF0aWMvbWVkaWEvY3Jvc3MuOTRmOGZmOTAucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig10]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBpbWFnZXMvaW5kZXgvYXBwLXVpLXNjcmVlbnNob3QtZmlsZS1tYW5hZ2VyLnBuZy4gVHJ5IGVpdGhlciBhbiBhYnNvbHV0ZSBvciBhIHJlbGF0aXZlIHdpdGggYSB2YWxpZCByZWZlcmVyIFVSTC4gRm9ybWF0OiBodHRwczovL2ltYWdlcy53ZWIzLnN0b3JhZ2UvPE9QVElPTlM+LzxTT1VSQ0UtSU1BR0U+LiJ9fQ==
[fig11]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBpbWFnZXMvaW5kZXgvYXBwLXVpLXNjcmVlbnNob3QtZmlsZS11cGxvYWQucG5nLiBUcnkgZWl0aGVyIGFuIGFic29sdXRlIG9yIGEgcmVsYXRpdmUgd2l0aCBhIHZhbGlkIHJlZmVyZXIgVVJMLiBGb3JtYXQ6IGh0dHBzOi8vaW1hZ2VzLndlYjMuc3RvcmFnZS88T1BUSU9OUz4vPFNPVVJDRS1JTUFHRT4uIn19
[fig12]: data:application/json;base64,eyJvayI6ZmFsc2UsImVycm9yIjp7ImNvZGUiOiJIVFRQX0VSUk9SIiwibWVzc2FnZSI6IkltYWdlIFVSTCBpcyBpbnZhbGlkOiBpbWFnZXMvaW5kZXgvdGVzdGltb25pYWwtcnlhbi13LmpwZy4gVHJ5IGVpdGhlciBhbiBhYnNvbHV0ZSBvciBhIHJlbGF0aXZlIHdpdGggYSB2YWxpZCByZWZlcmVyIFVSTC4gRm9ybWF0OiBodHRwczovL2ltYWdlcy53ZWIzLnN0b3JhZ2UvPE9QVElPTlM+LzxTT1VSQ0UtSU1BR0U+LiJ9fQ==
