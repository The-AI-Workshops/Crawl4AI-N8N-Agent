# Crawl4AI & n8n Integration

A simple setup to run Crawl4AI locally, ingest into Qdrant, and wire up with n8n workflows.

---

## 🚀 Prerequisites

- **Docker & Docker Compose**  
- **n8n** with LangChain & Qdrant nodes  
- **Qdrant** instance up & running  
- API keys for:
  - `GROQ_API_KEY`
  - `OPENAI_API_KEY`
  - `ANTHROPIC_API_KEY`
  - `DEEPSEEK_API_KEY`
  - `TOGETHER_API_KEY`
  - `MISTRAL_API_KEY`
  - `GEMINI_API_TOKEN`

---

## 🛠️ Installation

```bash
git clone https://github.com/unclecode/crawl4ai.git
cd crawl4ai
cp .env.txt .llm.env
````

---

## 🔧 Configuration

Edit `.llm.env` and populate your keys:

```dotenv
GROQ_API_KEY="YOUR_GROQ_API"
OPENAI_API_KEY="YOUR_OPENAI_API"
ANTHROPIC_API_KEY="YOUR_ANTHROPIC_API"
DEEPSEEK_API_KEY=""
TOGETHER_API_KEY=""
MISTRAL_API_KEY=""
GEMINI_API_TOKEN=""
```

---

## ▶️ Start Services

```bash
docker compose up -d
```

---

## 📥 n8n Workflow Setup

1. **Import** the two JSON workflows in your n8n Editor:

   * `Crawl4AI___Single_URL_Webhook_Raw.json`
   * `Crawl4AI_Agent.json`
2. In the **Webhook** node (`/crawl4ai_single`):

   * Attach your **Header Auth** credential.
3. In **Generate Crawl4AI TaskId** node:

   * Update the HTTP **URL** to your local Crawl4AI endpoint if different.
4. In **Request Response** node:

   * Adjust the **URL** or timeout settings as needed.
5. For both workflows, select your **Qdrant** collection.
6. **Activate** both workflows.

---

## 🎯 Usage

* **Single URL scrape**
  POST to your webhook endpoint:

  ```bash
  curl -X POST http://<n8n-host>:<port>/crawl4ai_single \
    -H "Authorization: <your-header-token>" \
    -H "Content-Type: application/json" \
    -d '{ "url": "https://example.com" }'
  ```
* The agent will scrape the page, split & embed text, and upsert into your Qdrant DB.

---

## 📄 License

Released under the MIT License.
