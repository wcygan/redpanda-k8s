# Initial Installation

I was able to follow the instructions in the [RedPanda Kubernetes documentation](https://docs.redpanda.com/current/deploy/deployment-option/self-hosted/kubernetes/local-guide/) to install RedPanda on Kubernetes using Helm (without the RedPanda Operator).

```bash
skaffold run
No tags generated
Starting test...
Starting deploy...
Helm release redpanda not installed. Installing...
NAME: redpanda
LAST DEPLOYED: Sun Jan  5 16:24:36 2025
NAMESPACE: redpanda
STATUS: deployed
REVISION: 1
NOTES:
Congratulations on installing redpanda!

The pods will rollout in a few seconds. To check the status:

  kubectl -n redpanda rollout status statefulset redpanda --watch

Set up rpk for access to your external listeners:
  kubectl get secret -n redpanda redpanda-external-cert -o go-template='{{ index .data "ca.crt" | base64decode }}' > ca.crt
  rpk profile create --from-profile <(kubectl get configmap -n redpanda redpanda-rpk -o go-template='{{ .data.profile }}') default

Set up dns to look up the pods on their Kubernetes Nodes. You can use this query to get the list of short-names to IP addresses. Add your external domain to the hostnames and you could test by adding these to your /etc/hosts:

  kubectl get pod -n redpanda -o custom-columns=node:.status.hostIP,name:.metadata.name --no-headers -l app.kubernetes.io/name=redpanda,app.kubernetes.io/component=redpanda-statefulset

Try some sample commands:

Get the api status:

  rpk cluster info

Create a topic

  rpk topic create test-topic -p 3 -r 3

Describe the topic:

  rpk topic describe test-topic

Delete the topic:

  rpk topic delete test-topic
Waiting for deployments to stabilize...
 - redpanda:deployment/redpanda-console is ready. [1/2 deployment(s) still pending]
 - redpanda:statefulset/redpanda is ready.
Deployments stabilized in 1.653 second
You can also run [skaffold run --tail] to get the logs
```

Next, let's follow the advice in the output and setup `rpk`:

```bash
kubectl get secret -n redpanda redpanda-external-cert -o go-template='{{ index .data "ca.crt" | base64decode }}' > ca.crt
rpk profile create --from-profile <(kubectl get configmap -n redpanda redpanda-rpk -o go-template='{{ .data.profile }}') default
rpk profile use default

kubectl get pod -n redpanda -o custom-columns=node:.status.hostIP,name:.metadata.name --no-headers -l app.kubernetes.io/name=redpanda,app.kubernetes.io/component=redpanda-statefulset

rpk cluster info
```

Actually, that doesn't seem to "just work", so I found an alternative:

```bash
kubectl exec redpanda-0 --namespace redpanda -- rpk cluster info
kubectl exec redpanda-0 --namespace redpanda -- rpk topic create test-topic -p 3 -r 3
kubectl exec redpanda-0 --namespace redpanda -- rpk topic describe test-topic
kubectl exec redpanda-1 --namespace redpanda -- rpk topic describe test-topic
kubectl exec redpanda-0 --namespace redpanda -- rpk topic delete test-topic
```

## Next, let's see if we can get rpk working locally

[Connect to Redpanda in Kubernetes](https://docs.redpanda.com/current/manage/kubernetes/networking/k-connect-to-redpanda/)

It looks like we need these two:

1. https://docs.redpanda.com/current/manage/kubernetes/networking/k-connect-to-redpanda/#connect-to-an-external-cluster
2. https://docs.redpanda.com/current/manage/kubernetes/networking/external/k-nodeport/

It says it requires external access, but I'm not yet familiar with ExternalDNS and I don't want to over-expose my cluster to the internet, so we will just not do this for now :)
