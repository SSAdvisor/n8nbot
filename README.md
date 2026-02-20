# 🤖 n8nbot

**A self-hosted personal AI assistant built entirely with [n8n](https://n8n.io) — inspired by [OpenClaw](https://github.com/openclaw/openclaw).**

n8nbot replicates the core functionality of OpenClaw (multi-channel AI assistant with tools, memory, and automation) using n8n's visual workflow engine as the backbone. No custom code runtime needed — just n8n, a database, and your preferred LLM.

---

## ✨ Features

- **Multi-Channel Messaging** — One AI brain across Telegram, Slack, Discord, WhatsApp, and custom webhooks
- **Conversational Memory** — Short-term buffer + long-term persistent history + semantic RAG retrieval
- **AI Agent with Tools** — LLM-powered agent that can search the web, control a browser, send emails, query databases, and more
- **Modular Skill System** — Each capability is a self-contained n8n sub-workflow, easy to add/remove/customize
- **Scheduled Automations** — Proactive tasks like daily briefings, reminders, and monitoring
- **Browser Control** — Headless Chrome integration for web scraping, screenshots, and form automation
- **Fully Self-Hosted** — Your data stays on your infrastructure. No cloud dependencies required.
- **Visual Workflow Editor** — Build and modify your bot's behavior in n8n's drag-and-drop UI

---

## 📁 Project Structure

```text
n8nbot/
├── docs/                  # Documentation, architecture diagrams, guides
├── n8n-workflows/         # Exportable n8n workflow JSON files
│   ├── core/              # Core agent workflows
│   ├── channels/          # Channel-specific trigger/response workflows
│   └── skills/            # Tool/skill sub-workflows
├── n8nbot/                # Configuration, scripts, and Docker setup
│   ├── docker-compose.yml # Full stack deployment
│   ├── .env.example       # Environment variable template
│   └── config/            # Configuration files
├── LICENSE                # MIT License
└── README.md              # You are here
```

---

## 🏗️ Architecture

```text
┌─────────────────────────────────────────────────────┐
│              Messaging Channels                      │
│   Telegram │ Slack │ Discord │ WhatsApp │ Webhook    │
└──────┬──────┬───────┬─────────┬──────────┬──────────┘
       │      │       │         │          │
       ▼      ▼       ▼         ▼          ▼
┌─────────────────────────────────────────────────────┐
│              Message Normalizer                      │
│   Standardizes input from all channels into a        │
│   common format: { channel, userId, text, threadId } │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│                 AI Agent Core                         │
│                                                      │
│   ┌─────────┐  ┌──────────┐  ┌───────────────────┐  │
│   │   LLM   │  │  Memory  │  │      Tools        │  │
│   │ Claude/ │  │ Buffer + │  │ Web Search        │  │
│   │ OpenAI/ │  │ Postgres │  │ Browser Control   │  │
│   │ Ollama  │  │ + Vector │  │ Email / Calendar  │  │
│   └─────────┘  └──────────┘  │ Database Query    │  │
│                              │ Code Execution    │  │
│                              │ HTTP Requests     │  │
│                              │ File Management   │  │
│                              └───────────────────┘  │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              Response Router                         │
│   Routes reply back to the originating channel       │
└─────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| **Automation Engine** | [n8n](https://n8n.io) (self-hosted) |
| **LLM** | Anthropic Claude / OpenAI / Ollama (configurable) |
| **Database** | PostgreSQL (conversation history + n8n backend) |
| **Vector Store** | Qdrant (semantic memory / RAG) |
| **Browser Automation** | Browserless (headless Chrome) |
| **Cache** | Redis (optional: rate limiting, session cache) |
| **Deployment** | Docker Compose |

---

## 🚀 Quick Start

### Prerequisites

- Docker & Docker Compose
- An API key for your preferred LLM (Anthropic, OpenAI, etc.)
- Bot tokens for your desired channels (Telegram, Slack, etc.)

### 1. Clone the Repository

```bash
git clone https://github.com/SSAdvisor/n8nbot.git
cd n8nbot/n8nbot
```

### 2. Configure Environment

```bash
cp .env.example .env
# Edit .env with your API keys and configuration
```

### 3. Start the Stack

```bash
docker compose up -d
```

### 4. Access n8n

Open `http://localhost:5678` in your browser and set up your n8n account.

### 5. Import Workflows

Import the workflow JSON files from `n8n-workflows/` into your n8n instance:

1. Go to **Workflows** → **Import from File**
2. Import the core agent workflow first
3. Import channel workflows for your desired platforms
4. Import skill workflows as needed

### 6. Configure Credentials

In n8n, set up credentials for:
- Your LLM provider (Anthropic, OpenAI, etc.)
- Your messaging platforms (Telegram Bot Token, Slack OAuth, etc.)
- PostgreSQL connection
- Qdrant connection
- Any other services your skills require

### 7. Activate & Chat

Activate your workflows and send a message to your bot on any configured channel!

---

## 📋 Roadmap

### Phase 1 — Foundation
- [ ] Docker Compose stack (n8n + Postgres + Qdrant + Browserless)
- [ ] Core AI Agent workflow with Claude
- [ ] Telegram channel integration
- [ ] Window buffer memory
- [ ] Basic web search tool

### Phase 2 — Memory & Tools
- [ ] Persistent conversation history (Postgres)
- [ ] Vector store RAG memory (Qdrant)
- [ ] Email tool (Gmail / SMTP)
- [ ] Calendar tool (Google Calendar)
- [ ] Browser control tool (Browserless)
- [ ] Code execution tool

### Phase 3 — Multi-Channel
- [ ] Slack integration
- [ ] Discord integration
- [ ] WhatsApp integration (via WhatsApp Business API)
- [ ] Message normalizer / response router pattern
- [ ] Unified user identity across channels

### Phase 4 — Advanced
- [ ] Scheduled daily briefings
- [ ] Proactive notifications & reminders
- [ ] File management (upload/download/summarize)
- [ ] Image generation tool
- [ ] Smart home integration (Home Assistant)
- [ ] Custom skill template for community contributions

---

## 🔧 Adding a New Skill

Skills are modular n8n sub-workflows. To add one:

1. Create a new workflow in n8n
2. Start with a **Manual Trigger** (for testing) or **Execute Sub-Workflow Trigger**
3. Build your skill logic using any n8n nodes
4. Export the workflow JSON to `n8n-workflows/skills/`
5. Register it as a tool in the core AI Agent workflow

See [`docs/creating-skills.md`](docs/creating-skills.md) for a detailed guide.

---

## 🤝 Contributing

Contributions are welcome! Whether it's new skills, channel integrations, documentation, or bug fixes:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-new-skill`)
3. Commit your changes (`git commit -m 'Add new skill: weather lookup'`)
4. Push to the branch (`git push origin feature/my-new-skill`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **[OpenClaw](https://github.com/openclaw/openclaw)** — The inspiration for this project
- **[n8n](https://n8n.io)** — The incredible open-source workflow automation platform that makes this possible
- **[Anthropic](https://anthropic.com)** — For Claude, the default LLM powering n8nbot

---

<p align="center">
  Built with ⚡ n8n — no code, full power.
</p>
