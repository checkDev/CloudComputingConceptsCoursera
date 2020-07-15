# MemberShip Protocol Implementation (Distributed Systems)

## Overview

All cloud computing systems need to run a membership protocol in order to detect process (node) failures, and to incorporate new and leaving nodes. 

This Project focusses on implementing such a Gossip membership protocol. The project has implementation of an emulated network layer (EmulNet) , which mocks thousand cluster nodes (peers) over a real network. 
The membership protocol implementation  sits above EmulNet in a peer-to-peer (P2P) layer, but below an App layer. Think of this like a three layer protocol stack with Application, P2P, and EmulNet as the three layers (from top to bottom).

The program is am emulation of a distributed system, along with some tests.

## Objective
Implementation of a working Membership Protocol in a distributed system.


## The Three Layers
The three-layerimplementation framework providing will allow  to run multiple copies of peers within one process running a single-threaded simulation engine. Here is how the three layers work.

### 2.1 Emulated Network: EmulNet
EmulNet provides the following functions that your membership protocol above should use:
1. void *ENinit(Address *myaddr, short port);
2. int ENsend(Address *myaddr, struct address *addr, string data);
3. int ENrecv(Address *myaddr, int (* enqueue)(void *, char *, int), struct timeval
       *t, int times, void *queue);
4. int ENcleanup();
  
ENinit is called once by each node (peer) to initialize its own address (myaddr). ENsend and ENrecv are called by a peer respectively to send and receive waiting messages. ENrecv enqueues a received message using a function specified through a pointer enqueue(). The third and fourth parameters (t and times) are unused for now. You can assume that ENsend and ENrecv are reliable (when there are no message losses in the underlying network). ENcleanup is called at the end of the simulator run to clean up the EmulNet implementation. These functions are provided so that they can later be easily mapped onto implementations that use TCP sockets.

### 2.2 Application
This layer drives the simulation. FilesApplication.{cpp,h} contain code for this.
During each period, some peers may be started up, and some may be caused to crash- stop. Most importantly, for each peer that is alive, the function nodeLoop() is called. nodeLoop() is implemented in the P2P layer (MP1Node.{cpp,h}) and basically receives all messages that were sent for this peer in the last period, as well as checks whether the application has any new waiting requests.


### 2.3 P2P Layer
The functionality for this layer is pretty limited at this time. Files MP1Node.{cpp,h} contain code for this. This is the layer responsible for implementing the membership protocol. As such this is where your code should be implemented. You can very well imagine the P2P layer can be extended to provide functionalities like file insert, lookup, remove etc.

#### Introduction: 
  Each new peer contacts a well-known peer (the introducer) to join the group. This is implemented through JOINREQ and JOINREP messages.  JOINREP messages  specify the cluster member list. The introducer does not  maintain a list of all peers currently in the system; a partial list of fixed size has been maintained.

### Membership: 
  The membership protocol that satisfies completeness all the time (for joins and failures), and accuracy when there are no message delays or losses (high accuracy when there are losses or delays). The implementation followed is gossip-style heartbeating protocol.
  

## Coding Language
C++ .
C++11 (gcc version 4.7 and onwards).


## Testing
To compile the code, run make.
To execute the program, from the program directory run: ./Application testcases/<test_name>.conf. The conf files contain information about the parameters used by your application:
MAX_NNB: val
SINGLE_FAILURE: val DROP MSG: val MSG_DROP_PROB: val
where MAX_NNB represents the max number of neighbors, SINGLE_FAILURE is a one bit 1/0 variable that setssingle/multifailurescenarios,MSG_DROP_PROB representsthemessagedropprobability(between0 and 1) and MSG_DROP is a one bit 1/0 variable that decides if messages will be dropped or not.
There is a grader script Grader.sh. It tests your implementation of membership protocol in 3 scenarios and grades each of them on 3 separate metrics. The scenarios are as follows:
1. Single node failure
2. Multiple node failure
3. Single node failure under a lossy network.
The grader tests the followingthings:
     i)  whether all nodes joined the peer group correctly,
    ii)  whether all nodes detected the failed node (completeness) 
and iii) whether the correct failed node was detected (accuracy). 
  Each of these is represented as configuration files inside the testcases folder.
