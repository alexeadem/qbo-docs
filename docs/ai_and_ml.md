##  Nvidia GPU Operator

NVIDIA GPU Operator plays a crucial role in enabling organizations to harness the power of NVIDIA GPUs for AI and machine learning workloads in Kubernetes environments, leading to faster innovation, improved model performance, and greater efficiency in AI deployments.

QBO Kubernetes Engine (QKE) offers unparalleled performance for any ML and AI workloads, bypassing the constraints of traditional virtual machines. By deploying Kubernetes components using Docker-in-Docker technology, it grants direct access to hardware resources. This approach delivers the agility of the cloud while maintaining optimal performance.

### Prerequsites

<!-- * [CLI](cli.md) Configuration  -->
|   Dependency	        |      Validated or Included Version(s)     | Notes 
|-----------|----------| |
|[Kubernetes](https://kubernetes.io/docs/home/) | [v1.25.11](https://github.com/kubernetes/kubernetes/tree/release-1.25) | |
|[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)|   [v1.14.3](https://github.com/NVIDIA/nvidia-container-toolkit/releases/tag/v1.14.3)       ||
|[NVIDIA GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/index.html) | [v23.9.1](https://github.com/NVIDIA/gpu-operator/releases/tag/v23.9.1) ||
|NVIDIA Driver   | [535.129.03](https://www.nvidia.com/download/driverResults.aspx/213194/en-us/) [546.01](https://www.nvidia.com/download/driverResults.aspx/216365/en-us/)||
|[NVIDIA CUDA](https://docs.nvidia.com/cuda/)| [12.2](https://developer.nvidia.com/cuda-12-2-0-download-archive)||
|OS | Linux, Windows 10, 11 (WSL2)| |

### qbot
[1. Install qbot](qbot)

#### 2. Run qbot
```bash
./qbot gpu-operator
```


### 1. Create K8s Cluster

> For this tutorial we are using `nvidia` as our cluster name
```bash
export NAME=nvidia  
```

> Get qbo version to make sure we have access to qbo API
```bash
qbo version | jq .version[]?
```

> Add a K8s cluster with image v1.25.11. See [Kubeflow compatibility](ai_and_ml?id=kubeflow)
```bash
qbo add cluster $NAME -i hub.docker.com/kindest/node:v1.25.11 | jq
```

> Get nodes information using qbo API

```bash
qbo get nodes $NAME | jq .nodes[]?
```

> Configure kubectl 
```bash
export KUBECONFIG=$HOME/.qbo/$NAME.cfg
```

> Get nodes with kubectl
```bash
kubectl get nodes
``` 


### 2. Deploy Nvidia GPU Operatator
#### 2.1 Linux
> Nvidia GPU Operatator helm chart

```bash
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia || true
helm repo update
helm install --wait --generate-name -n gpu-operator --create-namespace nvidia/gpu-operator --set driver.enabled=false

```

#### 2.2 Windows (WSL2)

#### 2.2.1 Add PCI Labels
```bash
for i in $(kubectl get no --selector '!node-role.kubernetes.io/control-plane' -o json | jq -r '.items[].metadata.name'); do
        kubectl label node $i feature.node.kubernetes.io/pci-10de.present=true
done
```

#### 2.2.2 Deploy Chart Templates
```bash
git clone https://github.com/alexeadem/qbot
cd qbot/gpu-operator
OUT=templates
kubectl apply -f $OUT/gpu-operator/crds.yaml
kubectl apply -f $OUT/gpu-operator/templates/
kubectl apply -f $OUT/gpu-operator/charts/node-feature-discovery/templates/
watch kubectl get pods

```


### 3. Deploy Vector Add 
```
cat cuda/vectoradd.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cuda-vectoradd
spec:
  restartPolicy: OnFailure
  containers:
  - name: cuda-vectoradd
    image: "nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubuntu20.04"
    resources:
      limits:
        nvidia.com/gpu: 1

```

```bash
kubectl apply -f cuda/vectoradd.yaml
```

### 4. Get Vector Add Logs

```bash
kubectl logs cuda-vectoradd
```
```
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```


## Kubeflow

Kubeflow plays a crucial role in democratizing AI by providing a unified platform that enables organizations to efficiently develop, deploy, and manage AI applications at scale.

QBO Kubernetes Engine (QKE) offers unparalleled performance for any ML and AI workloads, bypassing the constraints of traditional virtual machines. By deploying Kubernetes components using Docker-in-Docker technology, it grants direct access to hardware resources. This approach delivers the agility of the cloud while maintaining optimal performance.

> The following instructions use the upstream Kubeflow project with `platform-agnostic-multi-user-pns` pipelines.

### Prerequisites
#### Kubeflow v1.7.0 with Nvidia GPU support

|   Dependency	        |      Validated or Included Version(s)     | Notes
|-----------|----------|---|
|[Kubernetes](https://github.com/kubernetes/kubernetes/tree/v1.25.11) | v1.25.11 | |
|[Kubeflow](https://www.kubeflow.org/docs/releases/kubeflow-1.7/)   | v1.7.0   | The autoscaling/v2beta2 API version of HorizontalPodAutoscaler is no longer served as of v1.26.Migrate manifests and API clients to use the autoscaling/v2 API version, available since v1.23. All existing persisted objects are accessible via the new API v1.25 [HorizontalPodAutoscaler not found on minikube when installing kubeflow](https://stackoverflow.com/questions/76502195/horizontalpodautoscaler-not-found-on-minikube-when-installing-kubeflow)|
|OS | Linux, Windows 10, 11 (WSL2)| |


#### Kubeflow v1.8.0 with Nvidia GPU support  


|   Dependency	        |      Validated or Included Version(s)     | Notes
|-----------|----------|---|
|[Kubernetes](https://github.com/kubernetes/kubernetes/tree/v1.25.11) | v1.25.11 | |
|[Kubeflow](https://www.kubeflow.org/docs/releases/kubeflow-1.8/)   | v1.8.0   | [GPU Vendor not available error #7273](https://github.com/kubeflow/kubeflow/issues/7273)|
|OS | Linux, Windows 10, 11 (WSL2)| |

### qbot
#### [1. Install qbot](qbot)

#### 2. Run qbot
```bash
./qbot kubeflow v1.7.0
```


### [1. Install Nvidia GPU Operator](ai_and_ml?id=nvidia-gpu-operator)

### 2. Install Kubeflow
  
```bash
cd $HOME
git clone https://github.com/kubeflow/manifests.git"
cd manifests/"
```

```bash
git checkout v1.7.0"
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
while ! ./kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```

### 3. Configure Kubeflow
#### 3.1 Patch Deployment for DinD
> Once this finishes we also need to patch the Kubeflow Pipelines service to not use Docker, otherwise our pipelines will get stuck and report Docker socket errors. This happens because despite us using Docker the Docker docket isn’t made available inside the kind cluster. So from Kubeflow’s perspective we are using containerd directly instead of Docker.
```bash
./kustomize build apps/pipeline/upstream/env/platform-agnostic-multi-user-pns | kubectl apply -f -
watch kubectl get pods -A
```

<!-- > Node port
> CORS issue
```
kubectl patch svc istio-ingressgateway --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]' -n istio-system
``` -->

### 4. Access Kubeflow UI
> Port forward
```bash
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80

```
#### 4.1 Linux
> You can then open your browser and navigate to http://127.0.0.1:8080 and login with the default credentials 

#### 4.2 Windows (WSL2)

> Under Windows Subsystem for Linux (WSL) you can install Google Chrome to access the product page 
> 
```bash
wget -O $HOME/google-chrome-stable_current_amd64.deb https://dl.>google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install $HOME/google-chrome-stable_current_amd64.deb
```

```
wsl.exe -e google-chrome http://127.0.0.1:8080
```

#### 4.3 Login
> Default Credentials
> 
> username: `user@example.com`
> 
> password: `12341234`


<!-- ![kubeflow nvidia-smi](img/kubeflow_nvidia_smi.png) -->

## Related Content

[Unlocking AI & ML Metal Performance with QBO Kubernetes Engine (QKE) Part I - Deploying Nvidia GPU Operator](https://www.qbo.io/#/blog_part_1_nvidia_gpu_operator)

[Unlocking AI & ML Metal Performance with QBO Kubernetes Engine (QKE) Part II - Deploying Kubeflowr](https://www.qbo.io/#/blog_part_2_kubeflow)

[Running Kubeflow inside Kind with GPU support](https://jacobtomlinson.dev/posts/2022/running-kubeflow-inside-kind-with-gpu-support/)
