# RedPanda on Kubernetes

[Reference 1](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/k-production-deployment/)
[Reference 2](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/local-guide/)
[Helm Charts](https://github.com/redpanda-data/helm-charts)

```bash
kubectl kustomize "https://github.com/redpanda-data/redpanda-operator//operator/config/crd?ref=v2.3.5-24.3.2" \
    | kubectl apply --server-side -f -
```

```bash
helm repo add redpanda https://charts.redpanda.com
helm upgrade --install redpanda-controller redpanda/operator \
  --namespace redpanda \
  --create-namespace \
  --values redpanda-operator-values.yaml
```

```bash
kubectl --namespace redpanda rollout status --watch deployment/redpanda-controller-operator
```

```bash
kubectl apply -f redpanda-cluster.yaml --namespace redpanda
kubectl get redpanda --namespace redpanda --watch
kubectl get pod --namespace redpanda
```

Cleanup:

```bash
kubectl delete -f redpanda-cluster.yaml --namespace redpanda
helm uninstall redpanda-controller --namespace redpanda
kubectl delete crd clusters.redpanda.vectorized.io
kubectl delete crd consoles.redpanda.vectorized.io
kubectl delete namespace redpanda
```