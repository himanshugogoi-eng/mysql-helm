# MySQL Deployment on Kubernetes using Helm

This repository demonstrates how to deploy MySQL on a Kubernetes cluster using the [Bitnami Helm Chart](https://github.com/bitnami/charts/tree/main/bitnami/mysql). It includes configuration for exposing the MySQL service both **inside** and **outside** the cluster using a **NodePort**.

---

## 🧰 Prerequisites

- Kubernetes cluster (e.g., [Minikube](https://minikube.sigs.k8s.io))
- [Helm 3](https://helm.sh/docs/intro/install/)
- kubectl

---

## 📦 Files

| File                | Description                                |
|---------------------|--------------------------------------------|
| `mysql-values.yaml` | Custom Helm values for MySQL deployment    |

---

## 🚀 Steps to Deploy

### 1. Add Bitnami Repo

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

### 2. Install MySQL using Helm

```bash
helm install mymysql bitnami/mysql -f mysql-values.yaml
```

### 3. Verify Resources

```bash
kubectl get pods
kubectl get svc
```

---

## 🛠️ Accessing MySQL

### ✅ Inside Cluster

```bash
kubectl run -it mysql-client --image=mysql:8 --rm --restart=Never -- bash
```

Then inside:
```bash
mysql -h mymysql-mysql -uroot -p
```

### 🌐 Outside Cluster (Host machine)

Get Minikube IP:
```bash
minikube ip
```

Then connect:
```bash
mysql -h <minikube-ip> -P 32000 -uroot -p
```

---

## 🧾 `mysql-values.yaml` (example config)

```yaml
auth:
  rootPassword: my-secret-password

primary:
  service:
    type: NodePort
    nodePort: 32000
  persistence:
    enabled: false
```

> ⚠️ In production, enable persistence and use Kubernetes secrets.

---

## 🧹 Cleanup

To remove everything:

```bash
helm uninstall mymysql
```

---

## 📚 References

- [Bitnami MySQL Helm Chart](https://artifacthub.io/packages/helm/bitnami/mysql)
- [Kubernetes Services Docs](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Helm Docs](https://helm.sh/docs/)
