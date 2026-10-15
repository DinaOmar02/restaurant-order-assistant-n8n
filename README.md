# restaurant-order-assistant-n8n
AI-powered Telegram restaurant ordering assistant built with n8n and a local LLM (Ollama).

<img width="1671" height="768" alt="image" src="https://github.com/user-attachments/assets/08b19dcf-5035-410b-82de-f4ffb0f76672" />

## Features

- Chats with customers on Telegram and recommends sandwiches from the menu
- Replies in the customer's language (Arabic or English)
- Returns structured JSON: `message`, `is_chosen`, `name`, `price`, `ingredients`
- Logs each interaction as a row in Google Sheets
- Uses a local LLM (Ollama), with no paid AI API

| Tool | Purpose |
|---|---|
| n8n | Workflow automation |
| Telegram Bot API | User interaction |
| Ollama (Qwen2.5:7b) | Local language model |
| Basic LLM Chain | Prompting the model |
| Structured Output Parser | Enforcing a JSON schema |
| Google Sheets | Storing orders 

## Architecture

```
Telegram Trigger → Basic LLM Chain ─┬→ Send a text message (Telegram)
                     ├── Ollama Chat Model        └→ Append row in sheet (Google Sheets)
                     └── Structured Output Parser
```

1. **Telegram Trigger** receives the customer's message.
2. **Basic LLM Chain** sends it to the Ollama model together with the system prompt (menu, rules, language).
3. **Structured Output Parser** forces the answer into a fixed JSON schema.
4. **Send a text message** replies to the customer on Telegram.
5. **Append row in sheet** saves the structured data to Google Sheets.

## Setup

### Prerequisites
- [n8n](https://n8n.io) (self-hosted)
- [Ollama](https://ollama.com) with a model pulled:
```bash
  ollama pull qwen2.5:7b
```
