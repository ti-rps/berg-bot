# 🤖 BergBot - Automação Fiscal de E-mails

![Status](https://img.shields.io/badge/status-ativo-success)
![Python Version](https://img.shields.io/badge/python-3.9%2B-blue)
![License](https://img.shields.io/badge/licen%C3%A7a-MIT-informational)

Um robô inteligente e robusto desenvolvido em Python para automatizar completamente o fluxo de recebimento e processamento de documentos fiscais (XMLs) enviados por e-mail.

---

## ✨ Funcionalidades Principais

* **📥 Monitoramento Contínuo:** Conecta-se a uma caixa de e-mail via IMAP e verifica novos e-mails em intervalos de tempo configuráveis.
* **📂 Extração Multi-Fonte de XMLs:** O bot é capaz de encontrar e extrair arquivos `.xml` de três formas diferentes:
    * **Anexos diretos** no e-mail.
    * **Arquivos compactados** (`.zip`, `.rar`, `.tar`), incluindo a capacidade de extrair de arquivos compactados que estão dentro de outros.
    * **Links do Google Drive** compartilhados no corpo do e-mail.
* **↪️ Respostas Automáticas:** Ao processar com sucesso novos arquivos de um e-mail, envia uma resposta de confirmação para o remetente e todos os destinatários, mantendo a conversa organizada na *thread* original.
* **⚙️ Prevenção de Duplicidade:** Mantém um arquivo de log (`log_downloads.txt`) para garantir que cada arquivo seja baixado e processado apenas uma vez, mesmo que o e-mail seja lido novamente.
* **🗂️ Organização Centralizada:** Salva todos os arquivos XML extraídos em uma única pasta de destino, facilitando o acesso e a integração com outros sistemas.
* **📲 Notificações de Alerta via WhatsApp:** Em caso de interrupção manual ou erro crítico, o bot envia uma notificação instantânea para uma lista de números de telefone, garantindo que os administradores estejam sempre cientes do status da automação.

---

## ⚙️ Como Funciona

O fluxo de trabalho do BergBot é cíclico e projetado para ser resiliente:

1.  **Conexão:** O bot inicia e se conecta ao servidor de e-mail.
2.  **Busca:** Procura por e-mails que atendam aos critérios definidos (por padrão, e-mails não respondidos e recentes).
3.  **Análise Profunda:** Para cada e-mail encontrado, ele inspeciona o corpo e todos os anexos.
4.  **Extração:** Se encontra um anexo direto, arquivo compactado ou link do Google Drive válido, ele baixa o conteúdo e extrai todos os arquivos XML.
5.  **Armazenamento e Log:** Salva os novos XMLs na pasta de destino e registra suas identificações no arquivo de log.
6.  **Confirmação:** Se novos arquivos foram salvos, uma resposta automática é enviada.
7.  **Marcação:** O e-mail é marcado como "Lido" e "Respondido" no servidor.
8.  **Pausa:** O bot aguarda o intervalo definido e reinicia o ciclo.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3
* **Bibliotecas Padrão:** `imaplib`, `smtplib`, `email`, `os`, `re`, `zipfile`, `tarfile`
* **Bibliotecas Externas:**
    * `requests`: Para realizar downloads de links.
    * `rarfile`: Para descompactar arquivos `.rar`.
    * `pywhatkit`: Para o envio de notificações via WhatsApp.

---

## 🚀 Instalação e Configuração

Siga os passos abaixo para colocar o BergBot em funcionamento.

### 1. Pré-requisitos

* **Python 3.9** ou superior instalado.
* **UnRAR:** A biblioteca `rarfile` precisa do executável do UnRAR para funcionar.
    * [Baixe o UnRAR Command Line](https://www.rarlab.com/rar/unrarw32.exe) para Windows.
    * Extraia e coloque o arquivo `UnRAR.exe` na mesma pasta onde o script do bot será executado.
* Uma conta de e-mail (Gmail é recomendado) com o acesso IMAP ativado.
    * **Importante:** Para o login, você precisará gerar uma **"Senha de App"** na sua conta Google, e não usar sua senha principal. [Saiba como aqui](https://support.google.com/accounts/answer/185833).

### 2. Passos de Instalação

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/seu-usuario/seu-repositorio.git](https://github.com/seu-usuario/seu-repositorio.git)
    cd seu-repositorio
    ```

2.  **Instale as dependências do Python:**
    ```bash
    pip install requests rarfile pywhatkit
    ```

3.  **Adicione os arquivos necessários:**
    * Coloque o `UnRAR.exe` que você baixou na pasta principal.
    * Adicione a imagem `BergBot.jpeg` (logo para o e-mail de resposta) na mesma pasta.

### 3. Configurando o Bot

Abra o arquivo do script e edite o dicionário `CONFIG` na função `main()` com suas informações:

```python
def main():
    CONFIG = {
        # --- Credenciais do E-mail ---
        "EMAIL_USUARIO": "seu-email@gmail.com",
        "EMAIL_SENHA": "sua-senha-de-app-aqui", # Use a Senha de App!

        # --- Servidores (Padrão para Gmail) ---
        "IMAP_SERVIDOR": "imap.gmail.com",
        "SMTP_SERVIDOR": "smtp.gmail.com",
        "SMTP_PORTA": 587,

        # --- Configurações do Bot ---
        "ADMIN_EMAIL": "email-admin@dominio.com", # E-mail para notificação de erro
        "PASTA_DOWNLOAD": r"C:\Caminho\Para\Salvar\XMLs", # Use o caminho de rede se necessário
        "ARQUIVO_LOG": "log_downloads.txt",
        "INTERVALO_SEGUNDOS": 60, # Tempo de espera entre as verificações (em segundos)

        # --- Filtros de Busca de E-mail ---
        "FILTERS": {
            "UNSEEN": False,      # True para buscar apenas e-mails não lidos
            "SINCE_DAYS_AGO": 3   # Buscar e-mails dos últimos X dias
        },

        # --- Notificações via WhatsApp ---
        "ENABLE_WHATSAPP_NOTIFICATION": True, # Mude para False se não quiser usar
        "WHATSAPP_RECIPIENT_PHONES": [
            "+5573999998888", # Adicione os números com código do país e DDD
            "+5571988887777"
        ]
    }
    # ... resto do código