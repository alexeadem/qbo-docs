## Coffee & Tea Nginx Ingress Controller

### Prerequisites

In this demo we'll deploy a sample coffee & tea application with kubernetes ingress controller in qbo.

|   Dependency	        |      Validated or Included Version(s)     | Notes
|-----------|----------|---|
|[kubernetes ingress controller](https://github.com/kubernetes/ingress-nginx)| [v1.9.5](https://github.com/kubernetes/ingress-nginx/blob/controller-v1.9.5/deploy/static/provider/cloud/deploy.yaml)|[Supported Versions table](https://github.com/kubernetes/ingress-nginx?tab=readme-ov-file#supported-versions-table)|
|kubernetes | v1.27.3||


https://github.com/kubernetes/ingress-nginx?tab=readme-ov-file#supported-versions-table

### qbot
#### [1. Install qbot](qbot)

#### 2. Run qbot
```bash
./qbot nginx
```

### [1. Add cluster](cluster_ops)

### 2. Deploy the ingress controller 

```bash
/usr/bin/kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.5/deploy/static/provider/cloud/deploy.yaml
```

### 3. Deploy Coffee & Tea Sample App
```bash
/usr/bin/kubectl apply -f /home/alex/qbo-demo/coffee/ingress-nginx/cafe
```

### 4. Access Coffee & Tea App
#### 4.1 Patch service
> Set [externalTrafficPolicy](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip) to `Cluster`

```bash
 kubectl patch svc  ingress-nginx-controller -p '{"spec":{"externalTrafficPolicy":"Cluster"}}' -n ingress-nginx
 kubectl patch svc ingress-nginx-controller --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]' -n ingress-nginx
```
#### 4.2 Get Coffee & Tea App Endpoint

#### 4.2.1 Linux
```bash
NODEPORT=$(kubectl get svc -n ingress-nginx -o json | jq -r '.items[].spec.ports[]? | select( .port == 443) | select(.nodePort) | .nodePort')
NODELST=$(kubectl get nodes -o json | jq '.items[].status.addresses[] | select(.type=="InternalIP") | .address' | tr -d '\"' | tr '\n' ' ')
```

#### 4.2.2 Windows (WSL2)
```bash
kubectl patch svc ingress-nginx-controller --type='json' -p '[{"op":"replace","path":"/spec/type","value":"ClusterIP"}]' -n ingress-nginx
kubectl port-forward svc/ingress-nginx-controller -n ingress-nginx 9443:443
```

#### 4.3 Test Coffee & Tea App
> If the response code `200` the deployment was successful.

#### 4.3 Linux
```bash
for i in $NODELST; do
        curl -kv -H 'host: cafe.example.com' https://$i:$NODEPORT/coffee
done
```

#### 4.3.2 Windows (WSL2)
```bash
curl -k -m 3 --write-out '%{http_code}' --silent --output /dev/null -H 'host: cafe.example.com'  https://localhost:9443/tea
```

