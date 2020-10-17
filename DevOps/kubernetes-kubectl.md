<h2>Kubernetes</h2>

- [Kubectl](#kubectl)
  - [kubectl objects/api resources](#kubectl-objectsapi-resources)
  - [explain](#explain)
  - [exec](#exec)
  - [get](#get)
  - [describe](#describe)
  - [log](#log)
  - [config](#config)
  - [port-forward](#port-forward)
- [Status](#status)
- [Manage Compute Resource](#manage-compute-resource)
- [Good reads](#good-reads)

## Kubectl

Ref - https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

### kubectl objects/api resources

- get list of k8s objects/api-resources </br>
  `kubectl api-resources -o wide` or `-o name`

### explain

### exec

`kubectl exec -it pulsar-broker-0 /bin/bash -n pulsar`

- how to check api version of a resource </br>
  `kubectl explain pod`

### get

Get all resource for a namespace </br>
`kubectl get all --all-namespaces`

Watch the pod creations using `-w`</br>
`kubectl get pods -n pulsar -w` </br>

Get the list of images for a namespace or pod </br>
`kubectl get pods --namespace kube-system -o jsonpath="{..image}"`

### describe

### log

check log for a pod</br>
`kubectl log -f pod_name_id -n pulsar`

check log of an init container </br>
`kubectl logs my-pod -c my-initcontainer`

### config

display config file</br>
`kubectl config view`

default kube config file location </br>
`/Users/rvashish/.kube/config`

### port-forward

**Forward one or more local ports directly to a pod or pods under deployment or service.**

example </br>
`$ kubectl port-forward pulsar-pulsar-manager-767d5f5766-64qpw 9527:9527 -n pulsar`
or </br>
`kubectl port-forward service/myservice 5000 6000` this will forward to pods under this service

## Status

Waiting: PodInitializing

## [Manage Compute Resource](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)

> Originally k8s dont limit the use of resources by Pod instead it equally distribute the cpu. By default all containers get the same proportion of CPU cycles.

> Resource contraints passed to pod - container definitions are futher used by k8s in `docker run` command arguments as `--cpu-share` and `--memory`

**_The CPU/Memory units_**

CPU quota is defined in milicore units i.e. `300m`
1 core = 1000 millicore
0.1 core = 100 millicore

Memory is defined using decimal or binary notations. Decimal(K,M,G) Binary(Ki, Mi, Gi)
1G = 1000M/ 1Gi = 1024 Mi
We use binary notation more precise in kubernetes pod/container resource constraint definition.

**Example**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
    - name: db
      image: mysql
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
```

**How to check node capacity and allocated resources**

```bash
kubectl describe nodes node-id-name
```

**How to check if pod was killed due to resource constraints**

```bash
kubectl describe pod pod-name
```

Other than cpu and memory we can also use ephemeral and node level and cluster level extended resources.
Extended resources are out of k8s namespace. There are two steps required to use Extended Resources. First, the cluster operator must advertise an Extended Resource. Second, users must request the Extended Resource in Pods.

[Compute Resource Reference](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)

## Good reads

- [ClusterIP vs NodePort vs LoadBalancer vs Ingress](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0)
