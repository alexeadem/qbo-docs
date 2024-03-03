# User Guide
# Cluster Operations
## Add Cluster
> Create two new cluster `dev prod` cluster
```
qbo add cluster dev prod | jq

```

> Create new cluster `test` with `5` nodes
```
qbo add cluster test -n 5 | jq

```

## Stop cluster
> Stop cluster `test`. All nodes in cluster `test` will be stopped
```
qbo stop cluster test | jq

```
## Start cluster
> Start cluster `test`.

```
qbo start cluster test | jq

```
## Delete cluster
> Delete cluster `test`. Cluster will be deleted. Operation is irreversible  

```
qbo delete cluster test | jq

```

 > Delete `all` clusters.  


```
qbo delete cluster -A | jq

```

# Node Operations

## Stop Node
> Stops node with name `node-8a774663.localhost` `bfc61532`
```
qbo stop node node-123 node-567 | jq

```

## Start Node
> Starts nodes `bfc61532`

```
qbo start node node-123 | jq

```
## Add Node

> Add new a new node to cluster `test`

```
qbo add node test | jq

```

> Add new `2` new nodes to cluster `test`

```
qbo add node test -n 2 | jq

```
## Delete Node
> Scale cluster down by deleting node `node-8a774663.localhost`

```
qbo del node node-123 | jq

```
## Get Networks
> Get all cluster networks
```
qbo get networks -A
```
> Get `dev` and `prod` cluster networks
```
qbo get network dev prod
```
## Get Clusters
> Get all clusters
```
qbo get clusters -A
```

# kubectl configuration

> From `qbo` web terminal
```
export KUBECONFIG=/tmp/qbo/test.conf
kubectl get nodes
```