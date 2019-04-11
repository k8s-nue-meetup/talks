name: inverse
layout: true
class: center, middle, inverse

---
# Bootstrap a kubernetes cluster in 5 minutes or so
## Kubernetes Meetup - Nürnberg
### Oz Tiram, 10 April 2019
#### github.com/oz123

---
layout: false
class: left

# What is kubeadm?

 * a minimum viable Kubernetes cluster installer. 
 * Conform.
 * support upgrades, downgrades, bootstrap tokens and more.

---
# Installing kubeadm

 * Packages for all major Linux distributions.
 * Just do `yum install`,  `apt get` or so.

---
# Demo - quick installation

One the master node:

```
kubeadm init --pod-network 10.233.0.0/16
kubectl apply -f calico*yml
```

One the worker node:

```
kubeadm join 192.168.0.13:6443 --token cb0... --discovery-token-ca-cert-hash sha256:..
```

---
class: center, middle, inverse
 	
#     👏 👏 👏 👏 👏

---
# woah!

## you want to know what happend there?

### let's repeat in slow motion

```
kubeadm alpha <stage>
```
---
class: remark-small-code
# Customisation with config file

all (!) subcommands accept a configuration file in a yaml format.

```
apiVersion: kubeadm.k8s.io/v1alpha2
kind: MasterConfiguration
kubernetesVersion: v1.12.7
apiServerCertSANs:
- "${LOAD_BALANCER_DNS}
api:
    controlPlaneEndpoint: "${LOAD_BALANCER_DNS}:${LOAD_BALANCER_PORT}"
etcd:
  local:
    extraArgs:
      listen-client-urls: "https://127.0.0.1:2379,https://${HOST_IP}:2379"
      advertise-client-urls: "https://${HOST_IP}:2379"
      listen-peer-urls: "https://${HOST_IP}:2380"
      initial-advertise-peer-urls: "https://${HOST_IP}:2380"
      initial-cluster: "${CURRENT_CLUSTER}"
      initial-cluster-state: "${CLUSTER_STATE}"
    serverCertSANs:
      - ${HOST_NAME}
      - ${HOST_IP}
    peerCertSANs:
      - ${HOST_NAME}
      - ${HOST_IP}
networking:
    # This CIDR is a Calico default. Substitute or remove for your CNI provider.
    podSubnet: 10.233.0.0/16
apiServerExtraArgs:
  allow-privileged: "true"
```
---
# Finding information about kubeadm configuration

```
kubeadm config print-default
```
or:

 [godoc kubeadm](https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm)
---
class: center, middle, inverse

# Questions ?

## 🤔🤔🤔🤔🤔

---
class: center, middle, inverse
#  Thank you !

## 👍👍👍👍👍👍
