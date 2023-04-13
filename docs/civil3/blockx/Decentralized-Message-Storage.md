
# The Decentralized Storage  and Solution of Decentralized Chat Message

---

## Why decentralized storage?

1. Big file or data like image are too expensive to store in chain
2. The **performance** of chain are too low
3. Your cloud store won't collapse / be banned / be delete

### Node or API Provider
- Everything have its own price
- If we want to interact with chain, we need to maintain ourselves' `node`
- Or use the `API` from the provider, they have deployed a number of node for services:
	- `Infura`
	- `Alchemy`

![](attachments/Pasted%20image%2020230310230036.png)

---


## Decentralized storage?

> Device stored files distributed and decentralized by follow the  `DStorage` protocol


### IPFS
`The InterPlanetary File System`

- A protocol

![](attachments/Pasted%20image%2020230310225544.png)


---

- On web2,  people get data or contents by domain. Like `blog.xx.xx` , `docs.xx.xx` 
- On `ipfs`, you need  the `CID` (Content ID)  and through `IPNS` to find specific files.
- `CID`  (content-based address)  a series of hashed characters like wallet key

![](attachments/Pasted%20image%2020230310225633.png)

![](attachments/Pasted%20image%2020230310225901.png)


---

### Filecoin

- 基于 IPFS 协议的基础设施、储存网络
- Awarding people `FIL` token to promote people's enthusiasm of shared they storage and network bandwidth.
- Everyone can rent their device out for earning reward.



---

## Decentralized Chat

###   Whisper
- End to End connection
- Construct a common `publickey` to encode/decode msg

- Message encrypt and decrypt by symmetrical encryption algorithm, just like hash and unhash, but the `key`  that paired it the `key`

![](attachments/Pasted%20image%2020230311014625.png)

---
## Question

### Matrix

- Been mentioned moths ago
- With multiple server 

- difference
IPFS and Matrix are two different technologies that serve different purposes, but they can be used together to build decentralized applications.

IPFS (InterPlanetary File System) is a protocol and network designed to create a distributed file system that is both efficient and resilient. IPFS allows you to store and retrieve files in a decentralized way, making it a popular choice for decentralized storage applications. IPFS also provides content-addressable storage, which means that data can be retrieved based on its unique hash instead of a URL or location.

Matrix, on the other hand, is an open standard for decentralized communication. It provides a set of APIs for building real-time communication applications such as instant messaging, voice and video calls, and IoT communication. Matrix is designed to be interoperable, meaning that users on different networks can communicate with each other seamlessly.

- Like whisper: end-to-end encryption(E2EE)
- But not the same  e2ee is a method form, whisper are a feature provided by EVM
> It is important to note that while E2EE provides additional security and privacy for messaging, it may impact some features and functionality, such as message search and message sync across multiple devices.

For example, for client A to send a message to client B, client A performs an HTTP PUT of the required JSON event on its homeserver (HS) using the client-server API. A’s HS appends this event to its copy of the room’s event graph, signing the message in the context of the graph for integrity. A’s HS then replicates the message to B’s HS by performing an HTTP PUT using the server-server API. B’s HS authenticates the request, validates the event’s signature, authorises the event’s contents and then adds it to its copy of the room’s event graph. Client B then receives the message from his homeserver via a long-lived GET request.

How data flows between clients:

```
    { Matrix client A }                             { Matrix client B }
        ^          |                                    ^          |
        |  events  |  Client-Server API                 |  events  |
        |          V                                    |          V
    +------------------+                            +------------------+
    |                  |---------( HTTPS )--------->|                  |
    |   homeserver     |                            |   homeserver     |
    |                  |<--------( HTTPS )----------|                  |
    +------------------+      Server-Server API     +------------------+
                          History Synchronisation
                              (Federation)
```

- Encryption
There are different ways to encrypt files before storing them on IPFS. One way is to use symmetric encryption, where the same key is used to encrypt and decrypt the data. Another way is to use asymmetric encryption, where a public key is used to encrypt the data and a private key is used to decrypt it.

[→Back](Blocx-Index.md)
