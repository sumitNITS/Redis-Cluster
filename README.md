# Redis-Cluster-Setup
This repository is for creating the Redis cluster environment in CentOs8
Before going into the actual cluster configuration, let me write little bit of theory here!

Redis Cluster is designed to support high levels of read and write operations and enables users to scale their database horizontally by adding more nodes to the cluster. In a Redis Cluster each node contains a subset of the keys and the cluster transparently handles the distribution of keys across the nodes. When a client connects to a Redis Cluster it is automatically redirected to the appropriate node based on the key it is trying to access. This allows the cluster to scale horizontally by adding additional nodes as the dataset grows. Redis Cluster also provides automatic failover and recovery. If a node goes down, the cluster will automatically detect the failure and promote one of the replica nodes to be the new primary for the keys owned by the failed node. This ensures that the cluster remains available even in the face of hardware or network failures. According to the Redis cluster documentation the minimal cluster that works as expected requires to contain at least 3 master nodes. For high availability it is recommended that one should have at least 6 nodes with three masters and three slaves, each master having a slave. Redis uses port 6379 (default) and the port 16379 is used for the cluster bus (a node-to-node communication channel using a binary protocol.

The configurations for redis cluster below are for 6 nodes (3 nodes will work as master nodes and 3 nodes will work as slave nodes)

## Prerequisites
* All servers with centos8 installation 
* Disable selinux
