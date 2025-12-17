## Deploying MetalLB

### 1. Install MetalLB

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.9/config/manifests/metallb-native.yaml
```

Wait for MetalLB to be ready

```bash
kubectl wait --namespace metallb-system \
  --for=condition=ready pod \
   --selector=app=metallb \
   --timeout=90s
```

### 2. Configure IP Address Pool

IMPORTANT: Choose an IP range that:

Is on the same subnet as your nodes (172.16.10.x or 172.16.30.x)
Doesn't conflict with existing IPs
Is routable from where you'll access the services

Edit the artifact and adjust the IP range, then apply:

```bash
kubectl apply -f metallb-setup.yaml
```

### 3. Verify Installation

Check MetalLB pods

```bash
kubectl get pods -n metallb-system
```

Check IP pool
```bash
kubectl get ipaddresspool -n metallb-system
```

Check L2 advertisement

```bash
kubectl get l2advertisement -n metallb-system
```

### 4. Deploy PostgreSQL with LoadBalancer Service
```bash
kubectl apply -f postgresql-deployment.yaml
```

### 5. Verify

Test with Your PostgreSQL Service

Once MetalLB is running, your postgresql-external service should get an IP

```bash
kubectl get service postgresql-external -n databases
```


Check MetalLB logs if still pending

```bash
kubectl logs -n metallb-system -l component=controller --tail=50
```