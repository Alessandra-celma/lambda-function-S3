# Executando Tarefas Automatizadas com Lambda Function e S3
## ⚙️ Tarefas Automatizadas com Amazon S3 e AWS Lambda

O **Amazon S3** e o **AWS Lambda** trabalham juntos para automatizar processos na nuvem sem necessidade de servidores.  
Sempre que ocorre uma ação em um bucket S3 (como o upload de um arquivo), o Lambda pode ser acionado automaticamente para executar uma tarefa específica.

---

###  Exemplos de Tarefas Automatizadas

|  **Tarefa** |  **Ação no S3** |  **O que a Lambda faz automaticamente** |
|----------------|------------------|-------------------------------------------|
| 1️⃣ Redimensionar imagens | Upload de imagem | Redimensiona e salva em outra pasta ou bucket |
| 2️⃣ Converter formatos de arquivo | Upload de `.png` | Converte para `.jpg`, `.webp`, ou outro formato |
| 3️⃣ Processar arquivos CSV | Upload de planilha | Lê os dados e grava no banco **DynamoDB** |
| 4️⃣ Analisar logs | Upload de arquivo de log | Verifica erros e envia alertas para o **CloudWatch** ou **SNS** |
| 5️⃣ Geração de miniaturas (thumbnails) | Upload de imagem | Cria versão menor e salva no mesmo bucket |
| 6️⃣ Backup e replicação automática | Upload de arquivo | Copia o arquivo para outro bucket ou região |
| 7️⃣ Compressão automática | Upload de arquivo grande | Compacta e salva a versão reduzida |
| 8️⃣ Notificações automáticas | Upload ou exclusão | Envia e-mail ou mensagem via **SNS** avisando sobre o evento |
| 9️⃣ Geração de metadados | Upload de documento | Extrai título, autor, tipo e grava em um arquivo **JSON** |
| 🔟 Exclusão automática | Remoção de arquivo | Aciona função Lambda para limpar dependências ou logs |

---

## Entendendo o Amazon S3 (Simple Storage Service)
O que é?
O Amazon S3 é o serviço de armazenamento em nuvem da AWS.
Ele permite guardar e acessar qualquer tipo de arquivo — imagens, vídeos, documentos, backups, logs, dados de sistemas — de forma segura, escalável e acessível pela internet.
Pense nele como um “HD infinito na nuvem”, que nunca enche e que você pode acessar de qualquer lugar. 

## Entendendo o AWS Lambda
O **AWS Lambda** é um serviço da Amazon que executa código **automaticamente**, sem precisar de servidores.  
Você apenas **cria uma função**, define **quando ela deve ser executada** (por exemplo, quando algo acontece no S3), e a AWS cuida de todo o resto.

---

### Conceito simples

- **Lambda = código que roda sob demanda**
- **Sem servidores:** você não precisa configurar nem manter máquinas.
- **Paga apenas quando executa.**

---

### Como funciona
**Exemplo:**  
Você envia uma imagem para o S3 → o Lambda é acionado automaticamente → redimensiona a imagem → salva a nova versão.

---

### Resumo rápido
- Executa código **sem servidor (serverless)**  
- Responde a **eventos automáticos** (S3, DynamoDB, API Gateway, etc.)  
- Ideal para **automações, processamento de arquivos e integrações rápidas**

##  Upload de Arquivos com Processamento e Registro no DynamoDB

Essa automação mostra como o **Amazon S3**, o **AWS Lambda** e o **Amazon DynamoDB** trabalham juntos para processar dados automaticamente.

---

###  Etapas explicadas

1️⃣ **Upload no S3**  
O usuário envia um arquivo CSV para o bucket S3.

2️⃣ **Evento automático**  
O S3 aciona o **Lambda** assim que o arquivo é armazenado.

3️⃣ **Processamento no Lambda**  
A função Lambda lê o conteúdo do CSV e transforma os dados.

4️⃣ **Registro no DynamoDB**  
Os dados processados são gravados automaticamente em uma tabela DynamoDB.

---

###  Benefícios
- Totalmente **automático**  
- **Sem servidores** para gerenciar  
- **Escalável** e de **baixo custo**  
- Ideal para pipelines de dados e relatórios  
##  Configurando a AWS Localmente com LocalStack
O **LocalStack** é uma ferramenta que permite **simular os serviços da AWS no seu computador**.  
Com ele, você pode criar e testar funções Lambda, buckets S3 e tabelas DynamoDB **sem precisar estar conectado à nuvem real da AWS**.
###  Por que usar o LocalStack?
✅ Evita custos reais na AWS  
✅ Permite testar localmente (modo offline)  
✅ Ideal para estudos e desenvolvimento de automações

---

###  Passos básicos de configuração

1️⃣ **Instale o Docker**  
O LocalStack roda dentro de um contêiner Docker.  
→ [Baixar Docker Desktop](https://www.docker.com/products/docker-desktop/)

2️⃣ **Instale o LocalStack**  
Use o comando:
```bash
pip install localstack
localstack start
aws configure
AWS Access Key ID: test
AWS Secret Access Key: test
Default region name: us-east-1
aws s3 ls --endpoint-url=http://localhost:4566
💡 Resumo rápido
	•	LocalStack = AWS local para testes
	•	Sem custos e sem internet
	•	Ideal para Lambda + S3 + DynamoDB em ambiente de desenvolvimento

```
---

Com o **LocalStack** em execução, você pode **criar buckets S3**, **enviar arquivos**, **simular funções Lambda** e **registrar dados no DynamoDB**, tudo localmente — sem precisar da AWS real.

---

###  1️⃣ Criando um Bucket S3 local

---

```bash
aws --endpoint-url=http://localhost:4566 s3 mb s3://meu-bucket-local
📦 Isso cria um bucket S3 dentro do ambiente LocalStack.

```
⸻

 2️⃣ Enviando um arquivo para o bucket
aws --endpoint-url=http://localhost:4566 s3 cp exemplo.csv s3://meu-bucket-local/
Agora o arquivo exemplo.csv está armazenado localmente dentro do bucket.

⸻

 3️⃣ Criando uma função Lambda simulada
```
Crie um arquivo simples chamado lambda_function.py:
def handler(event, context):
    print("Evento recebido:", event)
    return {"status": "processado com sucesso"}
Empacote o arquivo em um .zip:
zip function.zip lambda_function.py
E registre a função no LocalStack:
aws --endpoint-url=http://localhost:4566 lambda create-function \
  --function-name processaArquivoLocal \
  --runtime python3.9 \
  --role arn:aws:iam::000000000000:role/lambda-role \
  --handler lambda_function.handler \
  --zip-file fileb://function.zip
```

 5️⃣ Simulando o fluxo completo
 
	1.	Você envia um arquivo .csv para o bucket local do S3
	2.	O LocalStack gera um evento
	3.	A função Lambda local é executada
	4.	A Lambda grava um registro na tabela DynamoDB local
	
Tudo isso sem internet e sem custos reais 

   ## Resumo rápido
	•	🧱 S3 local: recebe arquivos
	•	⚡ Lambda local: processa os eventos
	•	🗃️ DynamoDB local: armazena os dados
	•	🔄 Tudo acontece dentro do seu computador (ambiente simulado)
