# **Fake-Shop - Pipeline CI/CD**

Este repositório contém a aplicação Flask **Fake-Shop** e uma pipeline CI/CD configurada com **GitHub Actions**. A pipeline automatiza o processo de **build da imagem Docker**, **push para o Docker Hub** e **deploy em um cluster Kubernetes**, utilizando os manifestos Kubernetes já fornecidos.

---

## **Pré-Requisitos**

Antes de executar a pipeline, verifique se você já configurou os seguintes itens:

1. **Docker Hub:**

   - Conta no [Docker Hub](https://hub.docker.com/).
   - Nome de usuário e senha do Docker Hub configurados como **Secrets** no repositório GitHub.

2. **Cluster Kubernetes:**

   - A pipeline espera que o cluster Kubernetes já esteja configurado e acessível via `kubectl`.

---

## **Configuração de Secrets no GitHub**

Para permitir que a pipeline funcione corretamente, configure os **Secrets** no repositório GitHub:

1. Vá até o repositório no GitHub.
2. Acesse: **Settings > Secrets and variables > Actions > New repository secret**.
3. Adicione os seguintes **Secrets**:
   - **DOCKER\_USERNAME**: Seu nome de usuário no Docker Hub.
   - **DOCKER\_PASSWORD**: Sua senha do Docker Hub.
   - **KUBECONFIG**: O conteúdo do seu arquivo de configuração Kubernetes (`~/.kube/config`).

     Para obter o conteúdo do seu `kubeconfig`, execute o seguinte comando no terminal:
     ```bash
     cat ~/.kube/config
     ```
     Copie e cole o conteúdo completo no Secret **KUBECONFIG**.

---

## **Executando a Pipeline CI/CD**

A pipeline será executada automaticamente sempre que você fizer **push** para a branch `main`.

### **Passos para Executar:**

1. **Faça alterações no código (se necessário)**.

2. **Commit e push das alterações para a branch **``**:**

   ```bash
   git add .
   git commit -m "Alterações no código da Fake-Shop"
   git push origin main
   ```

3. **Acompanhe a execução da pipeline no GitHub:**

   - Vá até a aba **Actions** do repositório e verifique o progresso da pipeline em tempo real.

---

## **O que a Pipeline Faz**

A pipeline automatiza as seguintes etapas:

1. **Build da Imagem Docker:**

   - A imagem é construída com base no `Dockerfile` existente no repositório.
   - Após o build, a imagem é publicada no Docker Hub com a tag `v1`.

2. **Deploy no Kubernetes:**

   - Os manifestos Kubernetes (incluindo Deployment e Services) são aplicados ao cluster usando o seguinte comando:
     ```bash
     kubectl apply -f deployment.yaml
     ```

---

## **Estrutura do Repositório**

Certifique-se de que o repositório contenha os seguintes arquivos e diretórios:

```
.
├── .github
│   └── workflows
│       └── cicd-pipeline.yml   # Arquivo da pipeline CI/CD do GitHub Actions
├── Dockerfile                  # Arquivo Docker para construir a imagem do app Flask
├── deployment.yaml             # Manifests Kubernetes para o deploy
├── requirements.txt            # Dependências do app Flask
├── index.py                    # Código principal do app Flask
└── README.md                   # Este arquivo de instruções
```

---

## **Configurações Técnicas**

### **Dockerfile**

O `Dockerfile` contém as instruções de construção da imagem Docker:

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY . .
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 5000
CMD ["python", "index.py"]
```

### **Manifests Kubernetes**

O arquivo `deployment.yaml` contém a configuração do Deployment e dos Services no Kubernetes.\
Esse arquivo inclui a configuração do banco de dados PostgreSQL e do app **Fake-Shop**.

---

## **Conclusão**

Agora, sua pipeline CI/CD está configurada e automatiza o build, push da imagem Docker e deploy no Kubernetes! 🚀\
Em caso de dúvidas, sugestões ou melhorias, sinta-se à vontade para abrir uma **issue** ou enviar um **pull request**.

---

**Autor:** João Caetano\
**Licença:** MIT

