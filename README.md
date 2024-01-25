# Implantação de uma aplicação no Kubernetes com Ingress utilizando o Kind

## Descrição da Aplicação

Este projeto é uma aplicação que utiliza o Kubernetes para implantar um serviço Redis e uma aplicação chamada Giropops Senhas. A aplicação Giropops Senhas é uma aplicação simples que gera senhas aleatórias. O serviço Redis é usado para armazenar essas senhas. Além disso, este projeto também demonstra como configurar o Nginx Ingress Controller para rotear o tráfego para a aplicação.

### Pré-requisitos:

- `kind` (v0.11.1 ou posterior) instalado em sua máquina. [Guia de Instalação](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- `kubectl` (v1.21 ou posterior) instalado e configurado para gerenciar o cluster. [Guia de Instalação](https://kubernetes.io/docs/tasks/tools/)
- `Docker` (v20.10.7 ou posterior) instalado e configurado. [Guia de Instalação](https://docs.docker.com/get-docker/)

---

### **1: Clone o Repositório**

```bash
git clone https://github.com/patrickmanzo/K8S-kind-NginxIngress-giropops-senhas.git
cd K8S-kind-NginxIngress-giropops-senhas.git
```

### **2: Crie o Cluster com `kind`**

Crie o cluster usando o arquivo `kind-com-ingress.yaml` com o comando:

```bash
kind create cluster --config kind-com-ingress.yaml
```

### **3: Instale o `Nginx Ingress Controller` no `kind`**

Instale o Ingress Controller executando o seguinte comando:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

### **4: Implantação da Aplicação**

#### **4.1: Deploy Redis**

Aplique o Deployment e o Service para o **Redis**:

```bash
kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml
```

#### **4.2: Deploy giropops-senhas**

Aplique o Deployment e o Service para a aplicação **Giropops Senhas**:

```bash
kubectl apply -f app-deployment.yaml
kubectl apply -f app-service.yaml
```

### **5: Acessando a aplicação sem o Ingress via `kubectl port-forward`**

Execute o seguinte comando para acessar a aplicação sem o Ingress usando o `kubectl port-forward`:

```bash
kubectl port-forward services/giropops-senhas 5000:5000
```

Abra o seu browser e acesse `http://localhost:5000`. 

Você deve ver a resposta do serviço `giropops-senhas`

### **6: Configurar o Ingress**

Aplique o arquivo de Ingress:

```bash
kubectl apply -f ingress-1.yaml
```

### **7: Verificar o Status do Ingress**

Execute os seguintes comandos para verificar se o Ingress foi criado corretamente:

```bash
kubectl get ingress
kubectl describe ingress giropops-senhas
```

#### **7.1: Testar o Acesso via Curl**

```bash
curl localhost
```

Este comando deve retornar a resposta do serviço `giropops-senhas` através do Ingress.

#### **7.2: Testar o Acesso via Browser**

Abra o browser e acesse `http://localhost`. Você deve ver a resposta do serviço `giropops-senhas` exibida no browser.

---

## Conclusão

Neste projeto, foi demonstrado o passo a passo a implantação de uma aplicação no Kubernetes utilizando o `kind`, incluindo a instalação do Nginx Ingress Controller para facilitar o roteamento do tráfego, através dos tópicos principais:

1. Criar um cluster Kubernetes utilizando o `kind`.
2. Instalar o NginxIngress Controller no cluster.
3. Implantar um serviço `Redis` e a aplicação `giropops-senhas`.
4. Acessar a aplicação sem o Ingress usando `kubectl port-forward`.
5. Configurar e testar o Nginx Ingress para roteamento de tráfego externo.

---

## Excluindo o Projeto

Depois de terminar de experimentar o projeto, você pode limpar os recursos que criou. Esses são os passos para realizar isso:

### **1: Excluir os Recursos da Aplicação**

Exclua os recursos da aplicação executando os seguintes comandos:

```bash
kubectl delete -f app-deployment.yaml
kubectl delete -f app-service.yaml
kubectl delete -f redis-deployment.yaml
kubectl delete -f redis-service.yaml
kubectl delete -f ingress-1.yaml
```

### **2: Excluir o Cluster do Kind**

Exclua o cluster do Kind executando o seguinte comando:

```bash
kind delete cluster
```