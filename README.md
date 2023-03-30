# Redis Cluster with HAProxy

Redis Cluster is designed to support high levels of read and write operations and enables users to scale their database horizontally by adding more nodes to the cluster. In a Redis Cluster each node contains a subset of the keys and the cluster transparently handles the distribution of keys across the nodes. When a client connects to a Redis Cluster it is automatically redirected to the appropriate node based on the key it is trying to access. This allows the cluster to scale horizontally by adding additional nodes as the dataset grows. Redis Cluster also provides automatic failover and recovery. If a node goes down, the cluster will automatically detect the failure and promote one of the replica nodes to be the new primary for the keys owned by the failed node. This ensures that the cluster remains available even in the face of hardware or network failures. According to the Redis cluster documentation the minimal cluster that works as expected requires to contain at least 3 master nodes. For high availability it is recommended that one should have at least 6 nodes with three masters and three slaves, each master having a slave. Redis uses port 6379 (default) and the port 16379 is used for the cluster bus (a node-to-node communication channel using a binary protocol.

The configurations for redis cluster below are for 6 nodes (3 nodes will work as master nodes and 3 nodes will work as slave nodes)

## Prerequisites

- All servers with centos8 installation 
- Disable selinux

### Follow the Configuration steps to create the cluster

1) Install redis on all the 6 nodes and start the service using the commands below

```bash
dnf module install redis
```

```bash
systemctl start redis
```

```bash
systemctl enable redis
```

```bash
systemctl status redis
```

2) Configure the redis.conf file which is under /etc/redis.conf

Edit the following configuration lines in redis.conf

```bash
bind  <IP_OF_YOUR_MACHINE>
```

```bash
protected-mode no
```

```bash
port 6379
```

```bash
cluster-enabled yes
```

```bash
cluster-config-file nodes-6379.conf
```

```bash
cluster-node-timeout 15000
```

Once you are done with the config changes restart the redis service in all the machines using the command

```bash
systemctl restart redis
```

3) Open the port 6397 and 16379 on all the machines

```bash
firewall-cmd --zone=public --permanent --add-port=6379/tcp
```

```bash
firewall-cmd --zone=public --permanent --add-port=16379/tcp
```

```bash
firewall-cmd --reload
```

4) Create the cluster using command redis-cli --cluster create

```bash
redis-cli --cluster create <IP_OF_MASTER_1>:6379 <IP_OF_MASTER_2>:6379 <IP_OF_MASTER_3>:6379 <IP_OF_SLAVE_1>:6379 <IP_OF_SLAVE_2>:6379 <IP_OF_SLAVE_3>:6379 --cluster-replicas 1
```

![cluster-setup-centos8](https://user-images.githubusercontent.com/37767537/228921795-7450d2e3-0662-4dc1-a8e1-0fcd35a34c0c.png)

Here --cluster-replicas 1 means one replica per master and the first slave will replicates the first master and so on in that order.

Cheers, that's all it takes to setup the Redis Cluster.

You can also check my blog post Redis Explained in a Simple way
https://medium.com/@krsumit449/redis-explained-in-a-simple-way-aef2abc7c5de)

### Configure a HAProxy on top of Redis Cluster

To understand more about HAProxy and it's configurations, you can visit "ansible" present above!

