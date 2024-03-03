# Persistent Storage

## Let's Encrypt Certs Persistent Storage

> Persistent volume example

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: locus-ws
spec:
  replicas: 1
  selector:
    matchLabels:
      app: locus-ws
  template:
    metadata:
      labels:
        app: locus-ws
    spec:
      containers:
      - name: locus-ws
        image: registry.eadem.com/alex/locus-cloud/locus-ws:latest
        # command: ["/usr/bin/ws", "-l", "info"]
        volumeMounts:
        - name: etc-ws
          mountPath: "/etc/ws/"
          # subPath: "api.json"
          readOnly: true
        - name: volume
          mountPath: /tmp/locus/acme
        ports:
        - containerPort: 80
          name: http
        - containerPort: 443
          name: https
      imagePullSecrets:
        - name: regcred
      dnsConfig:
        options:
          - name: ndots
            value: "1"
      volumes:
      - name: etc-ws
        configMap:
          name: locus-ws-cm
      - name: volume
        persistentVolumeClaim:
          claimName: locus-ws-pvc

```

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: locus-ws-pvc
  labels:
    # insert any desired labels to identify your claim
    app: locus-ws-pvc
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      # The amount of the volume's storage to request
      storage: 2Gi
```