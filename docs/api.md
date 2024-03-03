# API


## Get nodes

> Request
```json
{
    "cmd": "get nodes --all",
    "uuid": "de66b27a-74f5-4413-b86d-1f9c25cd7007"
}
```

> Response
```json
{
    "nodes": [
        {
            "name": "control-f5047fd5.localhost",
            "id": "98650026d1dff267006d4ae520ff096ec4ac4a051de33abc5f805674337e22db",
            "image": "kindest/node:v1.27.2",
            "cluster": "alex",
            "state": "ready",
            "address": "172.18.0.3",
            "os": "Debian GNU/Linux 11 (bullseye)",
            "kernel": "6.3.5-200.fc38.x86_64",
            "user": "alex@qbo.io",
            "cluster_id": "718bef86-7dfe-46c0-bf4e-3dfea83c6c28"
        },
        {
            "name": "node-92c30394.localhost",
            "id": "593a3711349ce31617e5e458815679bb06214c3d91c2d9bdf46a7809f3c98f2d",
            "image": "kindest/node:v1.27.2",
            "cluster": "alex",
            "state": "ready",
            "address": "172.18.0.5",
            "os": "Debian GNU/Linux 11 (bullseye)",
            "kernel": "6.3.5-200.fc38.x86_64",
            "user": "alex@qbo.io",
            "cluster_id": "718bef86-7dfe-46c0-bf4e-3dfea83c6c28"
        },
        {
            "name": "node-4ba3bc48.localhost",
            "id": "0a3ee392e35304e5c30ef3b0d195aa138e62b10742ca9f626ee4e614b611ad7f",
            "image": "kindest/node:v1.27.2",
            "cluster": "alex",
            "state": "ready",
            "address": "172.18.0.6",
            "os": "Debian GNU/Linux 11 (bullseye)",
            "kernel": "6.3.5-200.fc38.x86_64",
            "user": "alex@qbo.io",
            "cluster_id": "718bef86-7dfe-46c0-bf4e-3dfea83c6c28"
        }
    ],
    "timestamp": 1686713251461070,
    "status": "OK",
    "code": 200,
    "command_id": "1ed73c50-26ee-47e1-ae3e-3cfad50d4c06",
    "command_line": "qbo get nodes -A",
    "command": "CMD_GET",
    "subcommand": "SUBCMD_NODE",
    "apicommand": "APICMD_NONE",
    "uuid": "de66b27a-74f5-4413-b86d-1f9c25cd7007",
    "user": "alex@qbo.io",
    "agent": "qbo-thread/cloud-stage-4.3.0-eae6ee8ce",
    "peer": "127.0.0.1",
    "host": "eadem.cloud.qbo.io",
    "source": "qbo.c:7147",
    "wsi": "0x55f86dc40840",
    "thread_id": 140279387227904,
    "vhost": "proxy"
} 
```

## Get images

> Request

```json
{
    "cmd": "get images",
    "uuid": "de66b27a-74f5-4413-b86d-1f9c25cd7007"
}
```
> Response
```json
{
    "images": [
        {
            "name": "hub.docker.com/kindest/node:v1.27.2",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.27.2"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.27.1",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.27.1"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.27.0",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.27.0"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.26.4",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.26.4"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.26.3",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.26.3"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.26.2",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.26.2"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.26.0",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.26.0"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.25.9",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.25.9"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.25.8",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.25.8"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.25.3",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.25.3"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.25.2",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.25.2"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.25.1",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.25.1"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.24.7",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.24.7"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.24.6",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.24.6"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.24.13",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.24.13"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.24.12",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.24.12"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.23.17",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.23.17"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.23.13",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.23.13"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.23.12",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.23.12"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.22.17",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.22.17"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.22.15",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.22.15"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.21.14",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.21.14"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.20.15",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.20.15"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.19.16",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.19.16"
        },
        {
            "name": "hub.docker.com/kindest/node:v1.18.20",
            "registry": "hub.docker.com",
            "user": "kindest",
            "repository": "",
            "image": "node",
            "tag": "v1.18.20"
        }
    ],
    "timestamp": 1686713551550960,
    "status": "OK",
    "code": 200,
    "command_id": "d8d0679b-37fd-41cc-b8f4-6d1c23785b67",
    "command_line": "qbo get images",
    "command": "CMD_GET",
    "subcommand": "SUBCMD_IMAGE",
    "apicommand": "APICMD_REGISTRY_AUTH",
    "uuid": "de66b27a-74f5-4413-b86d-1f9c25cd7007",
    "user": "alex@qbo.io",
    "agent": "qbo-thread/cloud-stage-4.3.0-eae6ee8ce",
    "peer": "127.0.0.1",
    "host": "eadem.cloud.qbo.io",
    "source": "qbo.c:7162",
    "wsi": "0x55f86da46e20",
    "thread_id": 140279387227904,
    "vhost": "proxy"
}
```

## Get networks

> Request
```json
{"cmd":"get networks --all","uuid":"de66b27a-74f5-4413-b86d-1f9c25cd7007"}
```

> Response
```json
{
    "networks": [
        {
            "name": "alex",
            "id": "718bef86-7dfe-46c0-bf4e-3dfea83c6c28",
            "subnet": "172.18.0.0/24",
            "state": "ready",
            "user": "alex@qbo.io"
        }
    ],
    "timestamp": 1686713809782984,
    "status": "OK",
    "code": 200,
    "command_id": "a709f917-f8b1-4365-acdc-53620316136d",
    "command_line": "qbo get networks --all",
    "command": "CMD_GET",
    "subcommand": "SUBCMD_NETWORK",
    "apicommand": "APICMD_NONE",
    "uuid": "de66b27a-74f5-4413-b86d-1f9c25cd7007",
    "user": "alex@qbo.io",
    "agent": "qbo-thread/cloud-stage-4.3.0-eae6ee8ce",
    "peer": "127.0.0.1",
    "host": "eadem.cloud.qbo.io",
    "source": "qbo.c:7024",
    "wsi": "0x55f86dff3420",
    "thread_id": 140279387227904,
    "vhost": "proxy"
}
```