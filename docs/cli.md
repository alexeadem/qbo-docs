
# Command Line Interface

> QBO is a client CLI that connects to the qbo API using websockets. It can send a single command as a message and disconnect. Or it can keep the websocket open to continue receiving state messages from the [mirror](?id=qbo-cloud). It runs in docker. 
## Configuration priority

QBO CLI processes configuration input in the following order:

1. Environment variables
* QBO_UID
* QBO_AUX
* QBO_HOST
* QBO_PORT
2. $HOME/.qbo/cli.json
3. $HOME/.qbo/.cli.db

## Download
> If you are accessing the cluster outside the `qbo` terminal you can install the CLI as follows:


```bash
git clone https://github.com/alexeadem/qbo-ce.git
cd qbo-ce
. ./alias
```
# Cluster Operations
## Add Cluster
> Create two new cluster `dev prod` cluster
```bash
qbo add cluster dev prod | jq

```

> Create new cluster `test` with `5` nodes
```bash
qbo add cluster test -n 5 | jq

```

## Stop cluster
> Stop cluster `test`. All nodes in cluster `test` will be stopped
```bash
qbo stop cluster test | jq

```
## Start cluster
> Start cluster `test`.

```bash
qbo start cluster test | jq

```
## Delete cluster
> Delete cluster `test`. Cluster will be deleted. Operation is irreversible  

```bash
qbo delete cluster test | jq

```

 > Delete `all` clusters.  


```bash
qbo delete cluster -A | jq

```

# Node Operations

## Stop Node
> Stops node with name `node-8a774663.localhost` `bfc61532`
```bash
qbo stop node node-123 node-567 | jq

```

## Start Node
> Starts nodes `bfc61532`

```bash
qbo start node node-123 | jq

```
## Add Node

> Add new a new node to cluster `test`

```bash
qbo add node test | jq

```

> Add new `2` new nodes to cluster `test`

```bash
qbo add node test -n 2 | jq

```
## Delete Node
> Scale cluster down by deleting node `node-8a774663.localhost`

```bash
qbo del node node-123 | jq

```

# Network operations
## Get Networks
> Get all cluster networks
```bash
qbo get networks -A
```
> Get `dev` and `prod` cluster networks
```bash
qbo get network dev prod
```
