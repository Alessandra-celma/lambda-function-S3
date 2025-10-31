# Executando Tarefas Automatizadas com Lambda Function e S3
## ⚙️ Tarefas Automatizadas com Amazon S3 e AWS Lambda

O **Amazon S3** e o **AWS Lambda** trabalham juntos para automatizar processos na nuvem sem necessidade de servidores.  
Sempre que ocorre uma ação em um bucket S3 (como o upload de um arquivo), o Lambda pode ser acionado automaticamente para executar uma tarefa específica.

---

### 💼 Exemplos de Tarefas Automatizadas

| 💼 **Tarefa** | 📦 **Ação no S3** | ⚡ **O que a Lambda faz automaticamente** |
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

### 🧩 Fluxo de Automação
