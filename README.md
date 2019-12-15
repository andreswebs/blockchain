# Blockchain exercise

This exercise intends to apply a distributed processing system for calculating hashcode's nonce.
Its goal is to use Go concurrency to apply an event-driven architecture to work the processing out.

## What should be done

### Heartbeat

The servers must stablish a connection where each one of them must send a Heartbeat Request and receive a Heartbeat Reply back each 5 seconds. Connection can be implemented in different networks via UDP or any other inter process communication protocol. In our case, we'll test locally through gRPC.

### Leadership

With a couple of servers up, a leadership must be set. The servers are going to have an order where the first one receives the identifier 1, second receives the identifier 2 and so on. The leader is the one that is up and has the lowest identifier. Each server has to have the same server relation and should calculate the same leadership each 5 seconds. If the actual leader is up, a new server becomes the leader if there is a server up.

### Mining

With the servers set, there are two papers: leader and node. The leader must do a request to a webservice to get the previous block and zeroes quantity. It will generate a timestamp and split a set range (1 to 2 billion, from 100.000 to 100.000) for the nodes to process.
In order to do that, the node must request processing when sending Heartbeat Reply with `ProcessRequest` message. Doing that, the leader must send the received hash, bitsize, timestamp and the range the node should process.
The node must o through the received range and combine the timestamp and the nonce to check if the hash generated through double SHA-256 starts with at least the zeroes received. The node will return a negative event (`ProcessAnswerNo`) or the nonce value for the valid hash back to the leader (`ProcessAnswerYes`).
The leader will send the nonce and timestamp back to the web service before requesting information about the next block (`Process`) and send a `ProcessInterrupt` message to the other processing nodes.

## Tasks

### Heartbeat

- [] Implement two servers initialization
- [] Implement server creation in main
- [] Store server address in a local environment
- [] Remove this local environment when the process is finished
- [] Implement heartbeat request each 5 seconds
- [] Implement heartbeat reply
- [] Unify the servers creation with local variable to set server quantity and main should call a `server` method to instantiate the server in the set quantity