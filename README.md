# Automação Inteligente de Triagem e Classificação de E-mails com n8n e Gemini AI

![n8n](https://img.shields.io/badge/n8n-Self--Hosted-orange?style=for-the-badge&logo=n8n)
![Docker](https://img.shields.io/badge/Docker-Desktop-blue?style=for-the-badge&logo=docker)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-1.5%20Flash-8E75B2?style=for-the-badge&logo=google)
![Gmail API](https://img.shields.io/badge/Gmail%20API-OAuth2-red?style=for-the-badge&logo=gmail)

Uma solução completa de automação *self-hosted* desenvolvida para triagem, análise contextual e rotulagem automática de e-mails em tempo real. O sistema utiliza a API do **Google Gemini** para interpretação semântica das mensagens e aplica marcadores dinâmicos no **Gmail** via credenciais OAuth2.

---

## Arquitetura do Fluxo no n8n

O fluxo é composto por 6 etapas principais em sequência:

```text
[Gmail Trigger] ➔ [Message a Model (Gemini)] ➔ [Code (Parse JSON)] ➔ [Get Many Labels] ➔ [Code (Mapeamento ID)] ➔ [Add Label to Message]
```

Descrição dos Nós:
Gmail Trigger (Message Received): Monitora continuamente a caixa de entrada para detectar novos e-mails.

Message a Model (Google Gemini): Analisa o assunto, remetente e corpo do e-mail, classificando a mensagem em uma categoria predefinida e retornando uma resposta estritamente estruturada em JSON.

Code in JavaScript (Parse JSON): Trata e limpa a resposta do Gemini (removendo marcadores de bloco de código) para extrair o campo categoria.

Get Many Labels (Gmail API): Consulta a API do Gmail dinamicamente para obter a lista atualizada de marcadores existentes e os respectivos IDs internos (Label_ID).

Code in JavaScript (Mapeamento ID): Cruza o nome da categoria gerada pela IA com a lista de marcadores reais do Gmail, garantindo o mapeamento correto do ID sem codificação manual.

Add Label to Message (Gmail API): Aplica exclusivamente a etiqueta correspondente ao e-mail analisado.

🛠️ Tecnologias Utilizadas
n8n (Self-Hosted): Motor de automação executado em um container local via Docker Desktop.

Docker & Docker Compose: Gerenciamento e persistência do ambiente de automação.

Google Gemini API: Modelo de linguagem para análise contextual do conteúdo dos e-mails.

Gmail API & OAuth2: Integração segura com a infraestrutura do Google Cloud Console.

JavaScript (Node.js): Manipulação de estruturas de dados e tratamento de exceções no n8n.

🚀 Desafios Técnicos Resolvidos
Resolução do Erro 403 (Access Denied) no OAuth2: Configuração detalhada da Tela de Consentimento OAuth no Google Cloud Console com inclusão de Test Users autorizados para o ambiente local (http://localhost:5678).

Mapeamento Dinâmico de IDs de Marcadores: A API do Gmail rejeita nomes de texto simples (ex: "Faculdade") ao aplicar marcadores via expressão. A arquitetura foi desenvolvida para consultar os marcadores em tempo real (Get Many Labels) e realizar a correspondência pelo ID interno do Google (Label_ID), evitando atribuições estáticas ou aplicação múltipla indevida de etiquetas.

Tratamento de Payload e Limites de API: Definição de limites de paginação (Limit: 100) para garantir que marcadores pessoais não fossem omitidos pelas categorias padrão do sistema Gmail.

📊 Impacto de Negócio e Métricas (Estimadas)
Economia de Tempo: ~1,5 a 2 horas semanais gastas em triagem manual e organização da caixa de entrada.

Tempo de Resposta: Redução do tempo de identificação de e-mails prioritários/urgentes para menos de 30 segundos após o recebimento.

Precisão de Classificação: >95% de assertividade na categorização automática usando o modelo Gemini 1.5 Flash.

🔧 Como Executar este Projeto Localmente
Pré-requisitos
Docker Desktop instalado.

Conta no Google Cloud Console com a Gmail API ativada.

API Key do Google AI Studio (Gemini).

1. Iniciar o n8n via Docker
Bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
2. Configurar o n8n
Acesse http://localhost:5678.

Importe o arquivo JSON deste workflow (workflow-projeto1.json).

Configure as credenciais de Google OAuth2 API (Gmail) e Google Gemini API Key.

Ative o fluxo clicando em Publish.

📝 Licença
Este projeto foi desenvolvido para fins de portfólio na trilha AI Builder.
