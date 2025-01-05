# RedPanda on Kubernetes

RedPanda is a Kafka-compatible streaming platform.

## References

- [Overview](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/k-deployment-overview/)
  - [Local Deployment](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/local-guide/)
  - [Production Deployment](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/k-production-deployment/)
  - [Store Data in PersistentVolumes](https://docs.redpanda.com/current/manage/kubernetes/storage/k-persistent-storage/)
  - [Topic Management](https://docs.redpanda.com/current/manage/kubernetes/k-manage-topics/)
- [Helm Charts](https://github.com/redpanda-data/helm-charts)
  - [Customize the Helm Charts](https://docs.redpanda.com/current/manage/kubernetes/k-configure-helm-chart/)
  - [RedPanda Helm Chart](https://docs.redpanda.com/current/reference/k-redpanda-helm-spec/)
  - [RedPanda Operator Helm Chart](https://docs.redpanda.com/current/reference/k-operator-helm-spec/)
  - [RedPanda Console Helm Chart](https://docs.redpanda.com/current/reference/k-console-helm-spec/)
  - [RedPanda Upgrades](https://docs.redpanda.com/current/upgrade/k-rolling-upgrade/)
  - [RedPanda Operator Upgrades](https://docs.redpanda.com/current/upgrade/k-upgrade-operator/)
- Miscellaneous
  - [rpk installation](https://docs.redpanda.com/current/get-started/rpk-install/)
  - [Introduction to rpk](https://docs.redpanda.com/current/get-started/intro-to-rpk/)
  
## Quickstart

Install the RedPanda operator CRDs:

```bash
kubectl kustomize "https://github.com/redpanda-data/redpanda-operator//operator/config/crd?ref=v2.3.5-24.3.2" \
    | kubectl apply --server-side -f -
```

Install the RedPanda operator:

```bash
helm repo add redpanda https://charts.redpanda.com
helm upgrade --install redpanda-controller redpanda/operator \
  --namespace redpanda \
  --create-namespace \
  --values redpanda-operator-values.yaml
```

Check the status of the operator:

```bash
kubectl --namespace redpanda rollout status --watch deployment/redpanda-controller-operator
```

Create a RedPanda cluster:

```bash
kubectl apply -f redpanda-cluster.yaml --namespace redpanda
```

Check the status of the cluster:

```bash
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