# RedPanda on Kubernetes

RedPanda is a Kafka-compatible streaming platform.

**Note**: I've chosen *not* to deploy the RedPanda Operator due to the additional complexity it has on installation (specifically how to integrate it with Skaffold). Instead, I will use the Helm charts directly.

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
