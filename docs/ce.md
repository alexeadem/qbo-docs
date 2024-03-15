# Community Edition (CE)

Create, deploy, and manage Kubernetes resources on your local machine with QBO Kubernetes Engine Community Edition (QKE CE). Deploy in Linux and Windows Subsystem for Linux (WSL2). 


# Installation

## Linux

#### Prerequisites
|   Dependency	        |      Validated or Included Version(s)     | Notes 
|------------|---|--|
|Docker API | 1.41 ||
|browser | Chrome or Firefox||
|cgroup | v2 ||
| max_user_watches| 2147483647||
| max_user_instances| 2048||
| max_queued_events| 2147483647||
| selinux| disabled||
| firewalld| inactive||

#### 1. Download
```bash
git clone https://github.com/alexeadem/qbo-ce.git
cd qbo-ce
```

#### 2. Configure
> You can use the `check_config` script to see if your system is ready for qbo

```bash
./check_config
OS = Linux
info: reading kernel config from /boot/config-6.6.4-100.fc38.x86_64 ...

- cgroup hierarchy: cgroup2fs
- /proc/sys/fs/inotify/max_user_watches: 2147483647
- /proc/sys/fs/inotify/max_user_instances: 2048
- /proc/sys/fs/inotify/max_queued_events: 2147483647
- selinux: disabled
- firewalld: inactive
- Docker: 1.43
```


##  Nvidia Runtime
> Support for Linux and Windows (WSL2)

#### 2.1 Configure Nvidia Runtime
```bash
cat /etc/docker/daemon.json
```

```json
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "args": [],
            "path": "nvidia-container-runtime"
        }
    }
}

```

```bash
cat ~/.qbo/api.json
{
"docker_runtime":"nvidia",
"registry_user":"kindest",
"registry_auth":"hub.docker.com",
"registry_token":"",
"registry_repo":"",
"registry_hostname":"hub.docker.com",
"registry_type":"docker"
}

```

##### 2.2 Test Nvidia GPU

```bash
docker run --gpus all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```

> Use `runc` for `docker_runtime` if Nvidia support is not needed.

#### 3. Start API

> Start the API
```bash
./qbo start api 
```

#### 3. Get Console Access
> QBO API listens on port 9601

##### 3.1 Linux
> To access the console you can navigate to
> 
```
http://localhost:9601
```

##### 3.2 Windows Subsystem for Linux (WSL2)
> In WSL2 you can find the WSL2 instance address as follows:
>
```
echo http://$(wsl -- ip -o -4 -json addr list eth0 | ConvertFrom-Json | %{ $_.addr_info.local }):9601
```
or
```
wsl hostname -I 
```


## Windows Subsystem for Linux (WSL2)

#### Prerequisites


|   Dependency	        |      Validated or Included Version(s)     | Notes 
|------------|---|--|
|Windows | 10, 11 ||
|WLSL | v2||
|cgroup | v2 ||


#### 1. Configure cgroup2fs 

> [ref. wslconfig](https://learn.microsoft.com/en-us/windows/wsl/wsl-config) 


```
C:\Users\<UserName>\.wslconfig.
```

> To get to your %UserProfile% directory, in PowerShell, use cd ~ to access your home directory (which is typically your user profile, C:\Users\<UserName>) or you can open Windows File Explorer and enter %UserProfile% in the address bar. The directory path should look something like: C:\Users\<UserName>\.wslconfig.

```
[wsl2]
kernelCommandLine = cgroup_no_v1=all
```



#### 2. Install WSL command
> [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

```

wsl --install

```
> Check which version of WSL you are running

```
wsl -l -v
```

#### 3. Docker Configuration

> Install Docker

```bash
sudo apt update && sudo apt upgrad
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2
update-alternatives --config iptables
. /etc/os-release
curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc
echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

```


> Start Docker

```bash
sudo usermod -aG docker $USER
newgrp docker
sudo dockerd
docker ps
```

> Test Docker
```
docker run hello-world
```




#### [4. Install qbo](#Installation)



# Console Access
> QBO API listens on port 9601

## Linux
> To access the console you can navigate to
> 
```
http://localhost:9601
```

## Windows Subsystem for Linux (WSL2)
> In WSL2 you can find the WSL2 instance address as follows:
>
```
echo http://$(wsl -- ip -o -4 -json addr list eth0 | ConvertFrom-Json | %{ $_.addr_info.local }):9601
```
or
```
wsl hostname -I 
```

# Registry Configuration

> Compatible with `Kind` images
[https://hub.docker.com/r/kindest/node/tags](https://hub.docker.com/r/kindest/node/tags)

## Docker
> To use default [Kind](https://kind.sigs.k8s.io/) images you can set the follwing configuration: 

```bash
cat << EOF > ~/.qbo/api.json
{
"registry_user":"kindest",
"registry_auth":"hub.docker.com",
"registry_token":"",
"registry_repo":"",
"registry_hostname":"hub.docker.com",
"registry_type":"docker"
}
EOF
```

## Gitlab

> To use [custom Kind images](custom_images.md) with `Gitlab` registries see configuration below:

```bash
cat << EOF > ~/.qbo/api.json
{
"registry_user":"alex",
"registry_auth":"git.gitlab.com",
"registry_token":"4Xo5X241L5vmsFSpkzXX",
"registry_repo":"qbo-ce",
"registry_hostname":"registry.gitlab.com",
"registry_type":"gitlab"
}
E0F

```


# Custom Kind Images

#### Requirements
|   Dependency	        |      Validated or Included Version(s)     | Notes
|-----------|----------|---|
|buildx|[v0.12.1](https://github.com/docker/buildx/releases/tag/v0.12.1)|[Linux packages](https://github.com/docker/buildx#linux-packages)


#### Install buildx

```bash
wget https://github.com/docker/buildx/releases/download/v0.12.1/buildx-v0.12.1.darwin-amd64
mv ~/Downloads/buildx-v0.9.1.linux-amd64 ~/.docker/cli-plugins/docker-buildx	
chmod +x ~/.docker/cli-plugins/docker-buildx

```



#### Image Script Usage

> `image` script can create custom kind images directly from the K8s source code and push the images to a Gitlab repo.

```bash
git clone https://github.com/alexeadem/qbo-ce.git
cd qbo-ce
./image 
./image list k8s                          - list all remote tags
./image list registry                     - list qbo nodes tags in registry
./image last {tag}                        - get lastest tags from https://github.com/kubernetes/kubernetes.git (default v1.18.19)
./image build base                        - build base image
./image build node {tag}                  - build node image
```

#### Image Script Configuration

```bash
vi image
```

> Replace the following values with your Gitlab configuration.
> 
> Eg. docker push registry.gitlab.com/alex1472/qbo-ce
```
### BEGIN GITLAB ###

REGISTRY=registry.gitlab.com
API=gitlab.com
OWNER=alex1476
REPO=qbo-ce

### END GITLAB ###
```
> [For more details refer to Gitlab docs](https://docs.gitlab.com/ee/user/packages/container_registry/index.html)


#### List Kubernetes Tags
> List k8s tags availale in Kubernetes upstream
```
./image list k8s
e8f0a6a19633ff704b78220666a54dce6d49942d	refs/tags/v1.26.0-alpha.1
1e4d7df2a59801e51c20fba94ad4153d270606f7	refs/tags/v1.26.0-alpha.0
3cb39a645e8ee19b3b121296399d065855d0457c	refs/tags/v1.25.3-rc.0
3229d1c27de5bc65b21139888f83b13c8e282f33	refs/tags/v1.25.2-rc.0
e313ec204d698755f1772515a87764bf524c8ac6	refs/tags/v1.25.2
8188504b9f2f08a1545f905733a9ec6820a62ee7	refs/tags/v1.25.1-rc.0
c3a6b2bc5f5a42fb3159df90f58980f22ce3cf4b	refs/tags/v1.25.1
...
```

#### List GitLab Registry Inages

> List all images available in the Gitlab repo

```
./image list registry
https://registry.eadem.com/v2/alex/qbo-ctl/node/tags/list
{"name":"alex/qbo-ctl/node","tags":["v1.17.17","v1.18.19","v1.19.11","v1.20.7","v1.21.1","v1.25.2"]}
```

#### Find Latest Tag

> Find the latest 1.25.x version

```
./image last 1.25
v1.25.2
```

#### Build Kind Base Image
> For more info see https://kind.sigs.k8s.io/docs/design/base-image/

```
./image build base
```

#### Build Kind Node 
> Example: Image with K8s version: v1.25.2
> For more info see https://kind.sigs.k8s.io/docs/design/node-image/

```
./image build node v1.25.2
```





