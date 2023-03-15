
# The Decentralized Storage  and Solution of Decentralized Chat Message


## Decentralized storage?

> Device stored files distributed and decentralized by follow the  `DStorage` protocol

![](attachments/Pasted%20image%2020230310225544.png)

### IPFS
`The InterPlanetary File System`

 On web2,  people get data or contents by domain. Like `blog.xx.xx` , `docs.xx.xx` 
 On `ipfs`, you need  the `CID`  and through `IPNS` to find specific files.
- `CID`  (content-based address)  a series of hashed characters like wallet key


![](attachments/Pasted%20image%2020230310225633.png)

![](attachments/Pasted%20image%2020230310225901.png)


### Make it available
- No free lunch in this world. Everything have its own price

![](attachments/Pasted%20image%2020230310230036.png)

### Filecoin

- Awarding people `FIL` token to promote people's enthusiasm of shared they storage and network bandwidth.
- User pay for storage service.
- Everyone can rent their device out to earn reward.

### Swarm


## Decentralized Chat

###  Swarm & Whisper
-  Encrypted message from a node to other node


- To implement  the decentralized  chat system is so hard,  more than the load of block chain, the  message security, etc.
- The whisper is a p2p protocol, so there is no intermediary server
- Message encrypt and decrypt by symmetrical encryption algorithm, just like hash and unhash, but the `key`  that paired it the `key`

![](attachments/Pasted%20image%2020230311014625.png)


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

[→Back](Blogx-Index.md)