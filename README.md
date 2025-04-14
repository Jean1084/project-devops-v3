# 🚀 Cluster Kubernetes avec Vagrant, VirtualBox et kubeadm

## 📁 Structure du projet

```text
k8s-cluster/
│
├── Vagrantfile
├── README.md
└── setup/
    ├── setup_project.sh
    ├── setup_master.sh
    ├── setup_node_1.sh
    └── setup_node_2.sh
├── manifests/
│   ├── app/
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── postgres/
│   │   ├── pvc.yaml
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── monitoring/
│   │   ├── prometheus-config.yaml
│   │   ├── prometheus-deployment.yaml
│   │   ├── prometheus-service.yaml
│   │   ├── grafana-deployment.yaml
│   │   └── grafana-service.yaml

```    
## 📦 Cluster Kubernetes (via kubeadm)

```text
│
├── 🧠 Master Node (control plane)
│   ├── kube-apiserver, kube-scheduler, controller-manager
│   └── etcd (ou externalisé si besoin de place)
│
├── 🔧 Worker Node 1
│   ├── Jeu web (Pod + Service)
│   └── DB (MySQL/PostgreSQL Pod)
│
├── 🔧 Worker Node 2
│   ├── Prometheus
│   └── Grafana
│
└── 🌐 Access via NodePort / Ingress (nginx)
```

## 🧾 Objectif
Ce projet permet de déployer un cluster Kubernetes local en utilisant Vagrant et kubeadm. Il est constitué de :

- 1 nœud master
- 2 nœuds workers
- Une configuration réseau avec Flannel
- Des scripts d'installation automatisés

## 🏗 Prérequis

- Vagrant
- VirtualBox
- Un PC Linux Ubuntu 24.04 avec au moins 8 Go RAM

## ⚙️ Installation

```bash
git clone <repo>
cd k8s-cluster
vagrant up
```