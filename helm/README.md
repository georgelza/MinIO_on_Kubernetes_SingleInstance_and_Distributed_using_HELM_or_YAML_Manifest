
## Deploy MinIO using Helm


### Add the MinIO Helm repository

```bash   
helm repo add minio https://charts.min.io/
helm repo update
```

### Install MinIO using Helm

```bash
helm install minio minio/minio -f standalone-values.yaml -n minio-helm-standalone --create-namespace
``` 

or

```bash
helm install minio minio/minio -f distributed-values.yaml -n minio-helm-distributed --create-namespace
```

### Verify deployment:

```bash
kubectl get svc -n minio-helm-standalone -o wide
kubectl get pods -n minio-helm-standalone
kubectl get pvc -n mminio-helm-standalone
kubectl get all -n mminio-helm-standalone
```

or

```bash
kubectl get svc -n minio-helm-distributed -o wide
kubectl get pods -n minio-helm-distributed
kubectl get pvc -n mminio-helm-distributed
kubectl get all -n mminio-helm-distributed
```

### Access MinIO:

```bash
#API:     http://172.16.10.155:9000
#Console: http://172.16.10.155:9001
```

or

```bash
#API:     http://172.16.10.156:9000
#Console: http://172.16.10.156:9001
```

### Login credentials:

```bash
#Username: mnadmin
#Password: mnpassword
```

### Uninstall:

```bash
helm uninstall minio -n minio-helm-standalone
```

or

```bash
helm uninstall minio -n minio-helm-distributed
```

### Access MinIO

```bash
kubectl port-forward svc/minio 9000:9000 -n minio-helm-standalone

kubectl port-forward service/minio-console 9001:9001 -n minio-helm-standalone
```

or

```bash
kubectl port-forward svc/minio 9000:9000 -n minio-helm-distributed

kubectl port-forward service/minio-console 9001:9001 -n minio-helm-distributed
```


## How to create buckets and access MinIO ?

### Creating Buckets and Accessing MinIO

Once you have MinIO running in Kubernetes, here’s how to create buckets and access your data:
Creating Buckets

You can create buckets in MinIO using several methods:

### Using the MinIO Console (Web UI)

Access the MinIO Console at:

- If using port forwarding: http://localhost:9001

- If using LoadBalancer/Ingress: Your configured URL

Log in with your credentials (mnadmin/mnpassword, note, the default username/password is minioadmin/minioadmin)

Click the “+” button in the bottom right

Select “Create Bucket”

Enter a bucket name and click “Create Bucket”


### Using the MinIO Client (mc)

First, set up the MinIO alias if you haven't already

```bash
mc alias set minio http://localhost:9000 mnadmin mnpassword
```

Create a bucket

```bash
mc mb minio/my-bucket
```

List buckets

```bash
mc ls minio
```

### Using Programming Languages

Python example:

```python

import boto3

# Create client
s3 = boto3.client('s3',
                  endpoint_url='http://localhost:9000',
                  aws_access_key_id='mnadmin',
                  aws_secret_access_key='mnpassword',
                  region_name='af-south-1',
                  config=boto3.session.Config(signature_version='s3v4'))

# Create bucket
s3.create_bucket(Bucket='my-bucket')

# List buckets
response = s3.list_buckets()
for bucket in response['Buckets']:
    print(bucket['Name'])
```


## Accessing MinIO Objects

Uploading Objects

### Using MinIO Console:

Navigate to your bucket

Click “Upload” button

Select files to upload


### Using MinIO Client:

Upload a file using mc

```bash
mc cp myfile.txt minio/my-bucket/
```

### Upload a folder using mc

```bash
mc cp --recursive myfolder/ minio/my-bucket/myfolder/
```