---
layout: post
title: Hivemind - A Decentralized and Distributed Database Concept
---

In this post, I want to layout and expand on my ideas for a distributed, decentralized database for application data. 

This stands in contrast to object stores, like Sia, IPFS, or Storj, and instead seeks to provide a decentralized data layer more similar to Firebase, but without a centralized system. 

# Why Decentralize? 

Decentralization is not the answer to everything. It has few use cases where the benefits of it outweight the costs. However, I think that app data fits one of those use cases. There are tradeoffs associated with decentralization, however I think that many would be okay with making that trade off if the benefits were clear.

Namely, the primary benefits of decentralization in this use case are: 
- Limited censorship 
- Higher availability
- Built-in redundancy
- Encryption and protection

### Problems and Drawbacks

Decentralization is not absent of negatives. In this use case, decentralization also has a few drawbacks. 

- Slower retrieval times 
- Slower write times
  - Reed-Solomon encoding
  - Storage agreement times
- More complicated development (leading to issues with hiring difficulties and costs)
- Scaling and growth complications 

In this, I'll attempt to provide solutions for as many of these drawbacks as I can. 

# Architecture 

### High Level Overview 

Hivemind will exist as essentially a GunDB instance for data persistence, with a Kad.js (Kademlia) networking layer for peer-to-peer interaction. 

GunDB has been chosen because it has a simple API, it's already been built to work well in a distributed nature, and it's incredibly fast, boasting performance of 10,000 writes per second. 

> Possible Alternative: I have considered [Clusterluck](https://www.npmjs.com/package/clusterluck) as well. 

These two technologies, when combined, should be able to provide most of the necessary functionality to bootstrap the network into existence. 

# Data Upload

Each node keeps a record of their neighbors storage capacities, which is updated periodically. This is called Capacity Check, and will be a tuneable metric. 

When that node wants to store data on that node, it will know how much storage each node in it's network has. It will cycle through nodes that have enough storage for their current request. 

> Possible issues: Gravity wells, node favoritism

# Reputation System 

Along with storage metrics, each node will also keep metrics on each neighbor node's other behaviors, allowing us to create a reputation system. 

Reputation of each node will be based on uptime, storage audits and challenges, network performance, storage capacity offered, and history of past transactions. 

> Possibly using Trust-Davis reputation system. However, trust-davis would require a financial incentive built in to the network.

# Data Download 

When a `get` request is sent, the node sends the retrieval request out to the network, and the node associated with it sends that data back. This creates an exchange event. These exchange events make up the history that reputation is also affected by. 


