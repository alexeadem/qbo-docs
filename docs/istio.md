## Istio Bookstore

### Prerequisites

In this demo we'll deploy a bookstore application with Istio in qbo.


|   Dependency	        |      Validated or Included Version(s)     | Notes
|-----------|----------|---|
|[istio](https://istio.io/latest/docs/setup/getting-started/) | 1.20.2|[Support status of Istio releases](https://istio.io/latest/docs/releases/supported-releases/#support-status-of-istio-releases)|
|kubernetes | v1.27.3||

### qbot
#### [1. Install qbot](qbot)

#### 2. Run qbot
```bash
./qbot istio
```


### [1. Add cluster](cluster_ops)

### 2. Download Istio
```bash
cd ~
```
```bash
curl -L https://istio.io/downloadIstio | sh -
```
```bash
cd $(ls -dt ~/istio* | head -1)
```

### 3. Install Istio
```bash
ISTIOCTL=$PWD/bin/istioctl
$ISTIOCTL install --set profile=demo -y --set meshConfig.defaultConfig.tracing.zipkin.address=splunk-otel-collector.istio-system.svc.cluster.local:9411
```
```bash
kubectl label namespace default istio-injection=enabled
```
### 4. Deploy Bookstore Sample App
```bash
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

### 5. Acesss Bookstore
#### 5.1 Deploy Istio gateway 
```bash
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

#### 5.2 Get External Adress

##### 5.2.3 Linux
```bash
NODEPORT=$(kubectl get svc -n istio-system -o json | jq -r '.items[].spec.ports[]? | select( .port == 80) | select(.nodePort) | .nodePort')
NODELST=$(kubectl get nodes -o json | jq '.items[].status.addresses[] | select(.type=="InternalIP") | .address' | tr -d '\"' | tr '\n' ' ')

for i in $NODELST; do

        echo "http://$i:$NODEPORT/productpage"

done
```

##### 5.2.4 Windows (WSL2)

```bash
kubectl patch svc istio-ingressgateway --type='json' -p '[{"op":"replace","path":"/spec/type","value":"ClusterIP"}]' -n istio-system
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
echo "http://localhost:8080/productpage"
```

> Under Windows Subsystem for Linux (WSL) you can install Google Chrome to access the product page 
> 
```bash
wget -O $HOME/google-chrome-stable_current_amd64.deb https://dl.>google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install $HOME/google-chrome-stable_current_amd64.deb
```

```
wsl.exe -e google-chrome http://localhost:8080/productpage
```
 



