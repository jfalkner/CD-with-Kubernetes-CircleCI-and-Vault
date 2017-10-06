# Kubernetes Example

These are example yaml files for deploying [controllers directly to k8s](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/). Helpful to see examples of direct k8s use.

```
# `create` the hello-node:v1 service. Can `delete` or `replace` as needed too
kubectl create -f ./hello-node.yaml

# deploy a cronjob that'll run every minute
kubectl create -f ./cronjob.yaml

# (optional) export out anything in k8s as yaml
kubectl get --export deployments -o yaml
```

Above works but it is better if you deploy everything via helm. If you don't, you'll end up manually managing a pile of files, such as what is in this directory.


