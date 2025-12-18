
## Various ways to Deploy MinIO on Kubernetes based cluster using HELM and YAML Manifest, Single Instance and Distributed HA

This started with my required for a [MinIO](https://www.min.io/) deployment as part of a larger lab build, and wanting to deploy it onto my [Kubespray](https://github.com/kubespray) based Kubernetes cluster vs previous local docker-compose service.

[Github](https://github.com/georgelza/MinIO_on_Kubernetes_SingleInstance_and_Distributed_using_HELM_or_YAML_Manifest.git)


### Requirements

- Kubernetes based cluster, mine was build using the [Kubespray](https://github.com/kubespray) deployment stack.

- CSI driver, I used Ceph RBD, see the [Deploying CEPH CSI (RBD & CephFS) onto Kubernetes])(https://medium.com/@georgelza/deploying-ceph-csi-rbd-cephfs-onto-kubernetes-6918a46cd8e1) for details.

- MetalLb Load balancer - see `metallb/README.md` for deployment.


You will see I demonstrate 2 deployment patterns:

- HELM based, (see `helm/README.md`)

- YAML/Manifest file based, (see `yaml/README.md`)

For each I then also show a single stand alone deployment and then a 4 node distributed deployment (note you will need at min 4 worker nodes in your K8s cluster, or need to modify the pod placement via the affinity block from `requiredDuringSchedulingIgnoredDuringExecution` to `preferredDuringSchedulingIgnoredDuringExecution`).


### CoreDNS

Also found I needed to modify my [coreDNS](https://coredns.io) configuration, required to add my cluster name (k8s-prd-1) and specified my internal edge router as primary local dns (located on 172.16.10.1).


```bash
cat <<EOF > coredns.yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: coredns
    namespace: kube-system
    data:
    Corefile: |
        .:53 {
            errors
            health {
                lameduck 5s
            }
            ready
            kubernetes k8s-prd-1 in-addr.arpa ip6.arpa {
                pods insecure
                fallthrough in-addr.arpa ip6.arpa
                ttl 30
            }
            forward . 172.16.10.1
            cache 30
            loop
            reload
            loadbalance
        }
EOF
```

```bash
kubeapply -f coredns.yaml
```


## Deployment

### Helm

For both the standalone and distributed deployment see `helm/README.md`.

### Yaml/Manifest file based

For both the standalone and distributed deployment see `yaml/README.md`.


