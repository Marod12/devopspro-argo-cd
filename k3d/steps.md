### Cria o cluster

```bash
k3d cluster create meucluster --servers 3 --agents 3 -p "8080:30000@loadbalancer"
```

### Verifica o `coredns`

```bash
kubectl -n kube-system get configmap coredns -o yaml
```

### Muda o DNS do Kubernetes

```bash
kubectl get configmap coredns -n kube-system -o yaml \
  | sed 's|forward . /etc/resolv.conf|forward . 1.1.1.1 8.8.8.8|' \
  | kubectl apply -f -
```

### Reinicia o `coredns`

```bash
kubectl rollout restart deployment coredns -n kube-system
```
