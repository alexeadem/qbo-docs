# Quick Start

### 1. Get version

```bash
qbo version | jq .version[]?
```
```json
{
  "qbo_cli": "dev-4.3.0-6efb3214c"
}
{
  "docker_api": "1.41",
  "docker_version": "20.10.23",
  "qbo_api": "cloud-dev-4.3.0-76da24342",
  "host": "lux.cloud.qbo.io"
}
```

### 2. Add cluster

```bash
qbo add cluster $USER -i hub.docker.com/kindest/node:v1.27.3 | jq
```

### 3. Get nodes

```bash
qbo get nodes $USER | jq .nodes[]?
```
```json
{
  "name": "control-2b4e4848.localhost",
  "id": "0c20025cd1947252881eaefbbb2ebbd31893c3263fd28826b766fc3ce4fecd7d",
  "image": "kindest/node:v1.28.0",
  "cluster": "alex",
  "state": "ready",
  "address": "172.18.0.3",
  "os": "Debian GNU/Linux 11 (bullseye)",
  "kernel": "6.4.4-200.fc38.x86_64",
  "user": "alex@qbo.io",
  "cluster_id": "0a3db628-d9d3-46a5-81d8-281e727d8e6c"
}
{
  "name": "node-69fad8d5.localhost",
  "id": "bce293a5bad56200f2c1574919b0a67ce9761e0ee3071dc7530c02fc12f3ae79",
  "image": "kindest/node:v1.28.0",
  "cluster": "alex",
  "state": "ready",
  "address": "172.18.0.4",
  "os": "Debian GNU/Linux 11 (bullseye)",
  "kernel": "6.4.4-200.fc38.x86_64",
  "user": "alex@qbo.io",
  "cluster_id": "0a3db628-d9d3-46a5-81d8-281e727d8e6c"
}
{
  "name": "node-c0dbf9c8.localhost",
  "id": "2b2c2e39140d1bc61577f16907c93f0703fcc45352433f18578701e3e4c3ec19",
  "image": "kindest/node:v1.28.0",
  "cluster": "alex",
  "state": "ready",
  "address": "172.18.0.5",
  "os": "Debian GNU/Linux 11 (bullseye)",
  "kernel": "6.4.4-200.fc38.x86_64",
  "user": "alex@qbo.io",
  "cluster_id": "0a3db628-d9d3-46a5-81d8-281e727d8e6c"
}
```

### 2. Get kubeconfig

#### 2.1 Get kubeconfig with qbot

#### [2.1.1 Install qbot](qbot)

#### 2.1.2 Run qbot 
```bash
./qbot kubeconfig $CLUSTER_NAME
```

#### 2.2 Get kubeconfig 

```bash
qbo get cluster alex -k | jq -r '.output[]?.kubeconfig | select( . != null)' > /home/alex/.qbo/$USER.cfg
export KUBECONFIG=/home/alex/.qbo/$USER.cfg
kubectl get nodes
NAME                         STATUS   ROLES           AGE   VERSION
control-2b4e4848.localhost   Ready    control-plane   17m   v1.28.0
node-69fad8d5.localhost      Ready    <none>          16m   v1.28.0
node-c0dbf9c8.localhost      Ready    <none>          16m   v1.28.0
```



