# vagrant-docker-hyperdex
Hyperdex cluster setup in vagrant using Docker as a provider

### 3 nodes cluster configuration example on one physical host:
```
 CLUSTER_SIZE = 3
 START_CLUSTER_ID = 1
 FROM_IP = "192.168.100.1"
 ALL_NODES_IN_CLUSTER = ["192.168.100.1","192.168.100.2","192.168.100.3"]
```

### 5 nodes cluster configuration example on one physical host:
```
CLUSTER_SIZE = 5
START_CLUSTER_ID = 1
FROM_IP = "192.168.100.1"
ALL_NODES_IN_CLUSTER = ["192.168.100.1","192.168.100.2","192.168.100.3","192.168.100.4","192.168.100.5"]
```

### 5 nodes cluster configuration example on two physical hosts:
```
Host 1 ("192.168.100.1","192.168.100.2","192.168.100.3"):
CLUSTER_SIZE = 3
START_CLUSTER_ID = 1
FROM_IP = "192.168.100.1"
ALL_NODES_IN_CLUSTER = ["192.168.100.1","192.168.100.2","192.168.100.3","192.168.100.4","192.168.100.5"]

Host 2 ("192.168.100.4","192.168.100.5"):
CLUSTER_SIZE = 2
START_CLUSTER_ID = 4
FROM_IP = "192.168.100.4"
ALL_NODES_IN_CLUSTER = ["192.168.100.1","192.168.100.2","192.168.100.3","192.168.100.4","192.168.100.5"]
```
