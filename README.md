# Prova 1 - DevOps

## Descrição

Este repositório contém o projeto da avaliação prática de DevOps.

### Componentes

* Aplicação desenvolvida em **FastAPI**.
* Arquivo de configuração **Kind** com 3 nós (1 `control-plane` e 2 workers).
* Workflow de **CI/CD** que:

  * Executa os testes automatizados.
  * Realiza o build da imagem Docker.
  * Publica a imagem no **Docker Hub**.
  * Atualiza a tag da imagem no `kustomization.yaml`.
  * Realiza commit e push da alteração.
* Arquivos de configuração **Kubernetes**, compatíveis com o **ArgoCD** para deploy automatizado da aplicação.

---

## Alterações Realizadas na Aplicação

Para que a aplicação FastAPI funcionasse corretamente, foram realizadas as seguintes alterações:

* Criado o arquivo `requirements.txt` com as dependências necessárias.
* Corrigido o endpoint `/square`.
* Implementada a lógica para que o **Uvicorn** execute a aplicação na porta `8000`.
* Por fim, para fins de teste, foi alterada a mensagem do endpoint `/` e do teste automatizado, garantindo que a execução das Actions não falhasse.

---

## Como Executar

### 1. Instalar o ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 2. Obter a senha inicial do ArgoCD

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d; echo
```

### 3. Expor o ArgoCD na porta 8080

```bash
kubectl port-forward svc/argocd-server -n argocd --address 0.0.0.0 8080:443
```

### 4. Configurar o ArgoCD

Crie uma aplicação no ArgoCD apontando para este repositório e utilize o caminho:

```
path: k8s
```

### 5. Expor a aplicação FastAPI

```bash
kubectl port-forward service/prova-1-devops-service --address 0.0.0.0 8000:80
```

---

## Observações

* Certifique-se de que o `kubectl` está configurado para o cluster criado com o Kind.
* O pipeline de CI/CD é executado automaticamente via GitHub Actions (ou o sistema definido no repositório).
* O ArgoCD detectará as alterações no repositório e aplicará automaticamente as atualizações no cluster.

---
