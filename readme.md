# RedPanda on Kubernetes

RedPanda is a Kafka-compatible streaming platform.

**Note**: I've chosen *not* to deploy the RedPanda Operator due to the additional complexity it has on installation (specifically how to integrate it with Skaffold). Instead, I will use the Helm charts directly.

## Quickstart

First, deploy RedPanda on Kubernetes:

```bash
skaffold run
```

Next, open the RedPanda Console

```bash
kubectl port-forward -n redpanda svc/redpanda-console 8080:8080
```

Then visit [http://localhost:8080/overview](http://localhost:8080/overview)

### Using rpk

It will helptul to create an alias for `rpk` since we are using it from a location that is external to the cluster. This alias will allow you to run `rpk` commands from your local machine.

```bash
alias internal-rpk="kubectl --namespace redpanda exec -i -t redpanda-0 -c redpanda -- rpk"
```

```bash
internal-rpk topic create twitch-chat
internal-rpk topic describe twitch-chat
```

Next, let's send some messages to the topic:

```bash
internal-rpk topic produce twitch-chat

heyo
Produced to partition 0 at offset 0 with timestamp 1736137280113.
what is up
Produced to partition 0 at offset 1 with timestamp 1736137283777.
:DDD
Produced to partition 0 at offset 2 with timestamp 1736137287104.
```

We can consume the messages from the topic using the following command:

```bash
internal-rpk topic consume twitch-chat --num 3
{
  "topic": "twitch-chat",
  "value": "heyo",
  "timestamp": 1736137280113,
  "partition": 0,
  "offset": 0
}
{
  "topic": "twitch-chat",
  "value": "what is up",
  "timestamp": 1736137283777,
  "partition": 0,
  "offset": 1
}
{
  "topic": "twitch-chat",
  "value": ":DDD",
  "timestamp": 1736137287104,
  "partition": 0,
  "offset": 2
}
```

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
  - [Troubleshooting the RedPanda Deployment](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/k-production-deployment/#troubleshoot)
  