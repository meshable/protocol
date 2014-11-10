**Attention**: This document is a WIP. If you found a bug or have a question please file an issue in this repo or come chat with us on [gitter](https://gitter.im/meshable/discussion)

Meshable Protocol
========

*For brevity, Bluetooth Low Energy is shortened to BLE*

1. [Meshable Node](#meshable-node)
  1. [Meshable Service](#meshable-service)
  2. [Meshable Characteristic](#meshable-characteristic)
  3. [Application Characteristic](#application-characteristic)
2. [Routing](#routing)

## Meshable Node

Each node on a Meshable network must be able to advertise (peripheral role) and connect (central role) over BLE. When a node connects to another node it can query whether the connected node is already a part of an existing mesh. If so, the new node will request the mesh's central to pull it into the mesh. If the central is unable to reach the new node to connect the new node can then bridge the mesh with its own connection.

Each Meshable node has its own unique 128-bit UUID used for identification, **not** to be confused with its service and characteristics UUID. (applications should have access to this id to utilize as needed) 
 
### Meshable Service

The Meshable service is how nodes on a Meshable network can identify and connect to each other. A Meshable service advertises with the UUID: `CF4A6676-F75D-47F0-861C-767CFBE35466`

### Meshable Characteristic

The Meshable characteristic held within the Meshable service holds information about the existing mesh network it belongs to if any. This characteristic advertises with the same UUID as its parent service (`CF4A6676-F75D-47F0-861C-767CFBE35466`)

### Application Characteristic
Apps built on the Meshable protocol will identify themselves with their own 128-bit UUID. These are used to create characteristics specific to the application and can scope interactions.

## Routing
Routing is a critical piece in any distributed system. Being able to send a message/packet to a specific node requires knowledge of the destination as well as any nodes to hop in between. The nature of Bluetooth Low Energy and the devices it resides on makes this difficult due to:

- Low range (nodes must be within 100m/300ft)
- Nodes are considered mobile and are often **not** on the network

Since nodes are likely to disappear from the network often and for extended periods creating a reliable routing table is next to impossible. This means effective message distribution will have to rely on flooding each node with the message in hopes of it arriving at its destination. As the message hops from node to node a TTL will be decremented until the message is discarded.

There is potential for *nicing* nodes who have seen the destination node before, giving them a much larger TTL and hopefully increasing the chances of the destination node seeing the message.

Acknowledgements are difficult as well. Do we fire and forget? Do we pass around a message saying the sent message was seen? How do we acknowledge the acknowledgement?


