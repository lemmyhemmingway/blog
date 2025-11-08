---
title: "Building a Multi-Node Kubernetes Cluster with Multipass"
date: 2025-11-08
image: "cover.webp"
tags: ["kubernetes", "ckad", "cka", "homelab", "multipass", "devops"]
categories: ["homelab", "Kubernetes", "multipass"]
---

## Introduction

When preparing for Kubernetes certifications like CKA, CKAD, or CKS, nothing beats real hands-on practice.  
But running multiple physical nodes isn’t always practical. Luckily, with Multipass — Canonical’s lightweight virtual machine manager — you can simulate a real multi-node cluster directly on your laptop.

In this guide, we’ll create a kubeadm-based Kubernetes cluster with one control-plane and two worker nodes, all running inside Multipass VMs on Ubuntu 24.04.


## Prerequisites

- A system with at least 8 GB RAM (12 GB recommended)  
- Multipass installed (snap install multipass or download from Canonical)  
- Basic command-line experience  


## Step 1 — Create the VMs

Create one master and two worker nodes:
```bash
multipass launch 24.04 --name master  --cpus 2 --mem 2G --disk 15G  
multipass launch 24.04 --name worker1 --cpus 1 --mem 1.5G --disk 10G  
multipass launch 24.04 --name worker2 --cpus 1 --mem 1.5G --disk 10G  
```

List and check IPs:

```bash
multipass list  
```

Example output:
```bash
Name      State    IPv4            Image  
master    Running  192.168.64.5    Ubuntu 24.04 LTS  
worker1   Running  192.168.64.6    Ubuntu 24.04 LTS  
worker2   Running  192.168.64.7    Ubuntu 24.04 LTS  
```


## Step 2 — Configure the Master Node

Enter the master shell:
```bash
multipass shell master
```

switch root
```bash
sudo -i  
```

Disable Swap and Enable Kernel Modules:
```bash
swapoff -a  
sed -i.bak '/ swap / s/^/#/' /etc/fstab  

modprobe overlay  
modprobe br_netfilter  

echo "overlay" | tee /etc/modules-load.d/k8s.conf  
echo "br_netfilter" | tee -a /etc/modules-load.d/k8s.conf  

cat <<EOF | tee /etc/sysctl.d/99-kubernetes-cri.conf  
net.bridge.bridge-nf-call-iptables  = 1  
net.bridge.bridge-nf-call-ip6tables = 1  
net.ipv4.ip_forward                 = 1  
EOF  

sysctl --system  
```

Install containerd:

```bash
apt-get update  
apt-get install -y containerd containernetworking-plugins  
mkdir -p /etc/containerd  
containerd config default | tee /etc/containerd/config.toml >/dev/null  
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml  
systemctl enable --now containerd  
```

Install kubeadm, kubelet and kubectl:
```bash
apt-get install -y apt-transport-https ca-certificates curl gpg  
mkdir -p /etc/apt/keyrings  
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg  

cat <<EOF | tee /etc/apt/sources.list.d/kubernetes.list  
deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /  
EOF  

apt-get update  
apt-get install -y kubeadm kubelet kubectl  
apt-mark hold kubeadm kubelet kubectl  
```

Initialize the Control Plane:

```bash
MASTER_IP=$(hostname -I | awk '{print $1}')  
kubeadm config images pull  
kubeadm init --apiserver-advertise-address=$MASTER_IP --pod-network-cidr=192.168.0.0/16  
```

Copy the join command printed at the end — you’ll use it for worker nodes.

Configure kubectl Access:
```bash
exit  
mkdir -p $HOME/.kube  
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
sudo chown $(id -u):$(id -g) $HOME/.kube/config  
```


## Step 3 — Install Calico CNI

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/calico.yaml  
kubectl get pods -n kube-system -w  
```

Wait for all pods (calico-node, calico-kube-controllers) to reach Running.  

Verify:  
```bash
kubectl get nodes -o wide  
```


## Step 4 — Configure the Worker Nodes

Repeat this process for both worker nodes:
```bash
multipass shell worker1   # or worker2  
sudo -i  
```

Prepare the Node:
```bash
swapoff -a  
sed -i.bak '/ swap / s/^/#/' /etc/fstab  

modprobe overlay  
modprobe br_netfilter  

echo "overlay" | tee /etc/modules-load.d/k8s.conf  
echo "br_netfilter" | tee -a /etc/modules-load.d/k8s.conf  

cat <<EOF | tee /etc/sysctl.d/99-kubernetes-cri.conf  
net.bridge.bridge-nf-call-iptables  = 1  
net.bridge.bridge-nf-call-ip6tables = 1  
net.ipv4.ip_forward                 = 1  
EOF  

sysctl --system  

apt-get update  
apt-get install -y containerd containernetworking-plugins  
mkdir -p /etc/containerd  
containerd config default | tee /etc/containerd/config.toml >/dev/null  
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml  
systemctl enable --now containerd  
```

Then install kubeadm, kubelet, and kubectl just like on the master.  

Finally, use the join command saved from master, for example:  

```bash
kubeadm join 192.168.64.5:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>  
```

Return to master and check:  
```bash
kubectl get nodes -o wide  
```

All nodes should now show Ready.  


## Wrapping Up

You now have a fully functional multi-node Kubernetes cluster running entirely on your laptop.  
This environment mirrors a production-grade setup and is perfect for learning, experimenting, and preparing for Kubernetes certification exams.


## Snapshot Tip

Before experimenting:  
```bash
multipass snapshot master  
multipass snapshot worker1  
multipass snapshot worker2  
```

To revert:  
```bash
multipass restore master  
```

Happy clustering!

