# Kubernetes

- [Kubernetes](#kubernetes)
    - [Supporting tools](#supporting-tools)
    - [CKAD References](#ckad-references)
    - [CKA Reference](#cka-reference)
    - [Others](#others)
    - [Best Practice](#best-practice)
      - [How to define namespace for pods](#how-to-define-namespace-for-pods)
    - [Blog Post](#blog-post)


### Supporting tools

- [cert-manager.io](https://cert-manager.io/)

### CKAD References

- [CKAD Exercises](https://github.com/dgkanatsios/CKAD-exercises)
- [Prep Notes](https://github.com/twajr/ckad-prep-notes)
- [Linux Academy Cheatsheet](https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf)
- [Tips and Tricks to pass CKAD](https://medium.com/chotot/tips-tricks-to-pass-certified-kubernetes-application-developer-ckad-exam-67c9e1b32e6e)
- [katacoda ckad](https://www.katacoda.com/fabito/scenarios/ckad)

### CKA Reference

- [CKA Walid Shaari](https://github.com/walidshaari/Kubernetes-Certified-Administrator)

### Others

### Best Practice 

[kubectl fails to port forward to ports <1024. All ports below 1024 require special permissions](https://stackoverflow.com/questions/53775328/kubernetes-port-forwarding-error-listen-tcp4-127-0-0-188-bind-permission-de)

#### How to define namespace for pods

| service               | namespace     | reference/reason                                                                                                                                                           |
| --------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aad-pod-identity`    | `kube-system` | [recommendation](https://github.com/Azure/aad-pod-identity#1-deploy-aad-pod-identity)                                                                                      |
| `kured`               | `kured`       | [azure docs](https://docs.microsoft.com/en-us/azure/aks/node-updates-kured#deploy-kured-in-an-aks-cluster)                                                                 |
| `signalfx`            | `org-system`  | could be any with correct privileges                                                                                                                                       |
| `fluentd`             | `kube-system` | [example on fluentd docs](https://docs.fluentd.org/v/0.12/articles/kubernetes-fluentd#requirements)                                                                        |
| `external-dns`        | `org-system`  | no recommendation                                                                                                                                                          |
| `agic`                | `org-system`  | no recommendation other than [one example that uses `default`](https://azure.github.io/application-gateway-kubernetes-ingress/troubleshooting/#verify-observed-nampespace) |
| `keyvault-csi-driver` | `kube-system` | no recommendation, but we feel it should be here                                                                                                                           |




- Kubernetes up and running book by - kelsey hightower

### Blog Post

- [Use kubectl to jumpstart a yaml file](https://blog.heptio.com/using-kubectl-to-jumpstart-a-yaml-file-heptioprotip-6f5b8a63a3ea)
