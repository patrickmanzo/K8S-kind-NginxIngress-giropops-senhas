# Deployment of an Application on Kubernetes with Ingress using Kind

## Application Description

This project is an application that uses Kubernetes to deploy a Redis service and an application called Giropops Senhas. The Giropops Senhas application is a simple service that generates random passwords, and the Redis service is used to store these passwords. Additionally, this project also demonstrates how to configure the Nginx Ingress Controller to route traffic to the application.

### Prerequisites:

- `kind` (v0.11.1 or later) installed on your machine. [Installation Guide](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- `kubectl` (v1.21 or later) installed and configured to manage the cluster. [Installation Guide](https://kubernetes.io/docs/tasks/tools/)
- `Docker` (v20.10.7 or later) installed and configured. [Installation Guide](https://docs.docker.com/get-docker/)

---

### **1: Clone the Repository**

```bash
git clone https://github.com/patrickmanzo/K8S-kind-NginxIngress-giropops-senhas.git
cd K8S-kind-NginxIngress-giropops-senhas
```

### **2: Install the `Nginx Ingress Controller` on `kind`**

Before proceeding, let's install the Nginx Ingress Controller, which will be responsible for routing external traffic to our application. Execute the following command:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

This command installs the Nginx Ingress Controller on your Kind cluster.

### **3: Create the Cluster with `kind`**

Create the cluster using the `kind-com-ingress.yaml` file:

```bash
kind create cluster --config kind-com-ingress.yaml
```

### **4: Application Deployment**

#### **4.1: Deploy Redis**

Apply the Deployment and Service for **Redis**:

```bash
kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml
```

#### **4.2: Deploy Giropops-Senhas**

Apply the Deployment and Service for the **Giropops Senhas** application:

```bash
kubectl apply -f app-deployment.yaml
kubectl apply -f app-service.yaml
```

### **5: Accessing the Application without Ingress via `kubectl port-forward`**

Execute the following command to access the application without Ingress using `kubectl port-forward`:

```bash
kubectl port-forward services/giropops-senhas 5000:5000
```

Open your browser and go to `http://localhost:5000`.

You should see the response from the `giropops-senhas` service.

### **6: Configure the Ingress**

Apply the Ingress file:

```bash
kubectl apply -f ingress-1.yaml
```

### **7: Check the Ingress Status**

Execute the following commands to verify if the Ingress was created correctly:

```bash
kubectl get ingress
kubectl describe ingress giropops-senhas
```

#### **7.1: Test Access via Curl**

```bash
curl localhost
```

This command should return the response from the `giropops-senhas` service through the Ingress.

#### **7.2: Test Access via Browser**

Open the browser and go to `http://localhost`. You should see the response from the `giropops-senhas` service displayed in the browser.

---

## Conclusion

In this project, we demonstrated the step-by-step deployment of an application on Kubernetes using `kind`, including the installation of the Nginx Ingress Controller to facilitate traffic routing.

---

## Deleting the Project

After finishing experimenting with the project, you can clean up the resources you created. Follow these steps to do so:

### **1: Delete Application Resources**

Delete the application resources by executing the following commands:

```bash
kubectl delete -f app-deployment.yaml
kubectl delete -f app-service.yaml
kubectl delete -f redis-deployment.yaml
kubectl delete -f redis-service.yaml
kubectl delete -f ingress-1.yaml
```

### **2: Delete the Kind Cluster**

Delete the Kind cluster by executing the following command:

```bash
kind delete cluster
```