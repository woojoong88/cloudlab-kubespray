# Kubespray for CloudLab (cloudlab-kubespray)

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/20065f2a383243648c9c5e44326ab32a)](https://app.codacy.com/app/woojoong88/cloudlab-kubespray?utm_source=github.com&utm_medium=referral&utm_content=woojoong88/cloudlab-kubespray&utm_campaign=Badge_Grade_Dashboard)

![GitHub](https://img.shields.io/github/license/woojoong88/cloudlab-kubespray.svg)
![GitHub top language](https://img.shields.io/github/languages/top/woojoong88/cloudlab-kubespray.svg)
![GitHub language count](https://img.shields.io/github/languages/count/woojoong88/cloudlab-kubespray.svg)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/woojoong88/cloudlab-kubespray.svg)
![GitHub repo size in bytes](https://img.shields.io/github/repo-size/woojoong88/cloudlab-kubespray.svg)
![GitHub All Releases](https://img.shields.io/github/downloads/woojoong88/cloudlab-kubespray/total.svg)

![GitHub issues](https://img.shields.io/github/issues-raw/woojoong88/cloudlab-kubespray.svg)
![GitHub closed issues](https://img.shields.io/github/issues-closed-raw/woojoong88/cloudlab-kubespray.svg)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/woojoong88/cloudlab-kubespray.svg)
![GitHub last commit](https://img.shields.io/github/last-commit/woojoong88/cloudlab-kubespray.svg)

Kubespray to deploy Kubernetes into CloudLab

## Preliminaries
### 1. Need to install Kubernentes and Helm on your local machine

### 2. Clone this source code into node1
```
node1$ git clone https://github.com/woojoong88/cloudlab-kubespray.git
```

## Install

### 1. Generate SSH Key for every machines
```
node1$ ssh-keygen -t rsa
node2$ ssh-keygen -t rsa
node3$ ssh-keygen -t rsa
...
```

### 2. Add node1's public key into other machine
```
node1$ cat ~/.ssh/id_rsa.pub
# Copy plain text in the id_rsa.pub file
# Access to node1 from node1
node1$ echo <paste plain text> >> ~/.ssh/authroized_keys
# Access to node2 from node1
node2$ echo <paste plain text> >> ~/.ssh/authroized_keys
# Access to node3 from node1
node3$ echo <paste plain text> >> ~/.ssh/authroized_keys

# Copy the key to each node
node1$ ssh <SSH ID>@node1
node1$ ssh-copy-id <SSH ID>@node1
node1$ ssh <SSH ID>@node2
node1$ ssh-copy-id <SSH ID>@node2
node1$ ssh <SSH ID>@node3
node1$ ssh-copy-id <SSH ID>@node3
```

### 3. Install virtualenv
```
node1$ sudo apt install virtualenv -y
```

### 4. Run setup.sh file
```
node1$ ./setup.sh -i <name> <ssh login ID> <Docker version> <node1 IP address> <node2 IP address> <node3 IP address>
```

**NOTE: Tested with Ubuntu 18.04 and Docker version 18.06, which is stable**

### 5. How to use Kubernetes with your local machine?
#### * Copy `admin.conf` file
```
node1$ sudo cat /etc/kubernetes/admin.conf
# Copy plain text in the admin.conf file
```

#### * Paste `admin.conf` file to your local machine
```
local$ vi <file name you want>.conf
# Then, paste the plain text you copied above
```

#### * Export configuration file you copied
```
local$ export KUBECONFIG=/path/to/admin.conf
```

#### * Validation
```
local$ kubectl get nodes
local$ kubectl get nodes
local$ helm ls
```

## Release information
* Release 1: [Mar. 5, 2019] Initial version -- Possible to set up K8S in CloudLab

* Master version: Might unstable

## Appendix 1. Troubleshooting

### * If one or more nodes are not ready yet
```
node1$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```

### * If get stuck at "Restart all kube-proxy pods to ensure that they load the new configmap"
```
node1 or node2$ export KUBECONFIG=/etc/kubernetes/admin.conf
node1 or node2$ sudo kubectl get pods --all-namespaces
# Get three kube-proxy pods
node1 or node2$ sudo kubectl delete pod <kube-proxy pod1> -n kube-system --force --grace-period=0
node1 or node2$ sudo kubectl delete pod <kube-proxy pod2> -n kube-system --force --grace-period=0
node1 or node2$ sudo kubectl delete pod <kube-proxy pod3> -n kube-system --force --grace-period=0
```
