# Deploy na AWS Lightsail 

Este guia descreve **o passo a passo necessário** para replicar o deploy do projeto em uma instância AWS Lightsail, incluindo:

- Criação da chave SSH para clonar repositórios privados  
- Instalação do Docker e Docker Compose  
- Instalação e configuração da AWS CLI  
- Integração com AWS Secrets Manager para variáveis de ambiente  
- Subida dos containers via Docker Compose  
- Script de deploy automatizado  

---

##  1. Criação da Instância AWS Lightsail

Para hospedar o projeto em um ambiente simples e estável, utilizamos o AWS Lightsail. Siga os passos abaixo:

### 1. Acessar o Lightsail
Acesse: https://lightsail.aws.amazon.com  
Clique em **Create instance**.

### 2. Configurar a instância
Selecione:

- **Platform:** Linux/Unix  
- **Blueprint:** OS Only → **Ubuntu 22.04 LTS** (ou 20.04)  
- **Instance plan:** escolha conforme a necessidade
- **Instance name:** algo como `weg-chat-prod`  

### 3. Criar a instância

Clique em **Create Instance** e aguarde o provisionamento.

### 4. Conectar via SSH

Via navegador:
- **Connect using SSH**


## 2. Criação do Secret no AWS Secrets Manager (ASM)

O ASM armazena as variáveis de ambiente do container Docker de forma segura.

### 1. Acessar o Secrets Manager
Acesse: https://console.aws.amazon.com/secretsmanager  
Clique em **Store a new secret**.

### 2. Selecionar tipo
- **Secret type:** Other type of secret  
- Clique em *Switch to plain text* e insira o conteúdo JSON:

```json
{
  "OPENAI_ENDPOINT": ,
  "OPENAI_MODEL_DEPLOYMENT_NAME": ,
  "OPENAI_MODEL_NAME":,
  "OPENAI_API_VERSION": ,
  "SECRET_OPENAI_API_KEY": ,
  "EMBEDDING_ENDPOINT":,
  "EMBEDDING_MODEL_DEPLOYMENT_NAME":,
  "EMBEDDING_MODEL_NAME":,
  "EMBEDDING_API_VERSION":,
  "SECRET_EMBEDDING_API_KEY":,
  "LANGSMITH_TRACING":,
  "LANGSMITH_ENDPOINT":,
  "LANGSMITH_API_KEY":,
  "LANGSMITH_PROJECT":,
  "OPENAI_API_KEY":,
  "SECRET_EMAIL_FROM":,
  "SECRET_EMAIL_TO":,
  "CREDENTIALS":,
  "GMAIL_TOKEN":

}
```

### 3. Nome do secret
Exemplos:

- `weg-chat-env-prod`  
- `backend-env-vars`  

### 4. Criar o secret
Clique em **Next** → **Next** → **Store**.

---

##  Criação do Usuário IAM para Acesso ao ASM

O usuário IAM será usado pela AWS CLI na instância Lightsail.

### 1. Criar usuário IAM
Acesse: https://console.aws.amazon.com/iam  
Clique em:

- **Users** → **Create User**  
- Nome: `lightsail-secrets-user`  
- Tipo de acesso: **Programmatic Access**

### 2. Conceder permissões

#### Política recomendada (mínima necessária)

Crie uma política chamada **SecretsManagerReadOnlyCustom**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "secretsmanager:DescribeSecret"
      ],
      "Resource": "*"
    }
  ]
}
```

Atribua essa política ao usuário.



### 3. Obter as chaves de acesso
Ao finalizar, a AWS exibirá:

- **AWS_ACCESS_KEY_ID**  
- **AWS_SECRET_ACCESS_KEY**

Baixe o arquivo `.csv`.

### 4. Configurar na instância Lightsail

Na máquina:

```bash
aws configure
```

Insira:

- Access Key ID  
- Secret Access Key  
- Region: `sa-east-1`  
- Output format: `json`

Agora a instância tem acesso ao Secrets Manager.

## 5. Gerar chave SSH na instância Lightsail

Gere a chave SSH dentro da  instância Lightsail:

```bash
ssh-keygen -t ed25519 -C "lightsail-deploy"
```

Quando perguntado sobre senha deixe vazio e pressione **Enter**.

Exiba a chave pública:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copie o conteúdo (uma única linha).

---

## 6. Adicionar a chave SSH ao GitHub

No GitHub:

1. Acesse **Settings**
2. Vá em **SSH and GPG Keys**
3. Clique **New SSH Key**
4. Nome: `Lightsail`
5. Cole a chave pública
6. Salve

Teste a conexão:

```bash
ssh -T git@github.com
```

Você deve ver:

```
Hi SEU_USUARIO! You've successfully authenticated...
```

---

## 7. Clonar o repositório via SSH

```bash
git clone git@github.com:SEU_USUARIO/SEU_REPO.git
cd SEU_REPO
```

---

## 8.  Instalar Docker e Docker Compose

```bash
sudo apt-get update && sudo apt-get upgrade -y
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker ubuntu
newgrp docker
sudo apt-get install -y docker-compose-plugin
```

---

## 9. Instalar AWS CLI

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt-get install -y unzip
unzip awscliv2.zip
sudo ./aws/install
aws configure
```

Configure com o usuário IAM que tem acesso ao Secrets Manager.

---

## 10. Subir containers com Docker Compose

```bash
docker-compose up -d --build
```

---


## 11. Checklist final

- Criar instância Lightsail  
- Criar chave SSH  
- Adicionar no GitHub  
- Clonar repositório  
- Instalar Docker  
- Instalar AWS CLI  
- Configurar IAM   
- Subir containers  

