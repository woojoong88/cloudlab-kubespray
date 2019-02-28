# Kubespray for CloudLab (cloudlab-kubespray)
Kubespray to deploy Kubernetes into CloudLab

[![Language]](https://img.shields.io/github/languages/top/woojoong88/cloudlab-kubespray.svg)

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
# Access to node2
node2$ echo <paste plain text> >> ./ssh/authroized_keys
# Access to node3
node3$ echo <paste plain text> >> ./ssh/authroized_keys
...
```

### 3. Install Docker Container
Follow the link: https://docs.docker.com/install/linux/docker-ce/ubuntu/

### 4. Install virtualenv
```
node1$ sudo apt install virtualenv python-pip -y
```

### 5. Run setup.sh file
```
node1$ ./setup.sh -i <name> <ssh login ID> <Docker version> <node1 IP address> <node2 IP address> <node3 IP address>
```

### 6. How to use Kubernetes with your local machine?
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
local$ helm ls
```

### Appendix 1. Troubleshooting

#### * If one or more nodes are not ready yet
```
node1$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```

