# CloudComputingConceptsCoursera

## Overview

All cloud computing systems need to run a membership protocol in order to detect process (node) failures, and to incorporate new and leaving nodes. 

This Project focusses on implementing such a Gossip membership protocol. The project has implementation of an emulated network layer (EmulNet) , which mocks thousand cluster nodes (peers) over a real network. 
The membership protocol implementation  sits above EmulNet in a peer-to-peer (P2P) layer, but below an App layer. Think of this like a three layer protocol stack with Application, P2P, and EmulNet as the three layers (from top to bottom).

The program is am emulation of a distributed system, along with some tests.

## Objective
Implementation of a working Membership Protocol in a distributed system.

## Coding Language
C++ is required for this assignment. The template code is written in C++. It is one of the most commonly used languages in industry for writing systems code. For this, you will need at least C++11 (gcc version 4.7 and onwards).
