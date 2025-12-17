
## Deploying MinIO using YAMl based manifest files.


## Deploy our single noded MINIO service

```bash
kubectl apply -f standalone-deployment.yaml
```

### Inspect deployment

```bash
# checkout PVCs
kubectl get pvc -n minio-yaml-standalone

# describe deployment
kubectl all -n minio-yaml-standalone -o wide

# describe deployment
kubectl describe pod/minio -n minio-yaml-standalone
```

### Access S3 service

http://172.16.10.157:9000

### Access Web console

http://172.16.10.157:9001



## Deploy our multi noded distributed MINIO service

kubectl apply -f distributed-deployment.yaml

### Inspect deployment

```bash
# checkout PVCs
kubectl get pvc -n minio-yaml-distributed

# describe deployment
kubectl all -n minio-yaml-standalone -o wide

# describe stateful set
kubectl describe statefulset.apps/minio -n minio-yaml-distributed
```

### Access S3 service

http://172.16.10.158:9000

### Access Web console

http://172.16.10.158:9001
