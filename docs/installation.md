
# Installation

## Linux

### Prerequisites
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

### 1. Download
```bash
git clone https://github.com/alexeadem/qbo-ce.git
cd qbo-ce
```

### 2. Configure
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


#### 2.1 Nvidia Runtime (Optional)
> Support for Linux and Windows (WSL2)

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

#### 2.2 Test Nvidia GPU

```bash
docker run --gpus all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```

> Use `runc` for `docker_runtime` if Nvidia support is not needed.

### 3. Start API

> Start the API
```bash
./qbo start api 
```

### 3. Get Console Access
> QBO API listens on port 9601

#### 3.1 Linux
> To access the console you can navigate to
> 
```
http://localhost:9601
```

#### 3.2 Windows (WSL2)
> In WSL2 you can find the WSL2 instance address as follows:
>
```
echo http://$(wsl -- ip -o -4 -json addr list eth0 | ConvertFrom-Json | %{ $_.addr_info.local }):9601
```
or
```
wsl hostname -I 
```


## Windows Subsystem for Linux (WSL)
### Prerequisites


|   Dependency	        |      Validated or Included Version(s)     | Notes 
|------------|---|--|
|Windows | 10, 11 ||
|WLSL | v2||
|cgroup | v2 ||


### 1. Configure cgroup2fs 

> [ref. wslconfig](https://learn.microsoft.com/en-us/windows/wsl/wsl-config) 


```
C:\Users\<UserName>\.wslconfig.
```

> To get to your %UserProfile% directory, in PowerShell, use cd ~ to access your home directory (which is typically your user profile, C:\Users\<UserName>) or you can open Windows File Explorer and enter %UserProfile% in the address bar. The directory path should look something like: C:\Users\<UserName>\.wslconfig.

```
[wsl2]
kernelCommandLine = cgroup_no_v1=all
```



### 2. Install WSL command
> [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

```

wsl --install

```
> Check which version of WSL you are running

```
wsl -l -v
```

### 3. Docker Configuration

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




### [4. Install qbo](#Installation)










