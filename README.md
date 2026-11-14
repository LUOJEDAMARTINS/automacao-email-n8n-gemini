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
