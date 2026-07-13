# Ticket Categorizer

**AI-powered support ticket classifier built with LangGraph and the Gemini API.**

🌐 **Live demo:** [https://ticket-categorizer.onrender.com/]

---

## What does this do?

When a customer sends in a support ticket whether through email or a web form, someone on the support team has to read it, figure out what it's about, and send it to the right person. At scale, that's a lot of manual work.

This project automates that first step. Paste in a support ticket, and the app will tell you:

- **What the issue is** (e.g. billing, login, delivery, security)
- **Which team should handle it** (e.g. billing team, security team)
- **How urgent it is** (low / medium / high / critical)
- **How the customer is feeling** (positive / neutral / negative / angry)
- **Whether a human should review it** before it gets routed
- **Whether any personal data was found** in the ticket (email, card numbers, etc.)

It also tells you exactly how confident it is in its answer, and shows you the cost of each API call down to 6 decimal places.

---

## Tech behind it

| Layer | Tool |
|---|---|
| AI pipeline | [LangGraph](https://github.com/langchain-ai/langgraph) — orchestrates the multi-step reasoning flow |
| AI model | [Gemini API](https://ai.google.dev/) (gemini-flash-latest) |
| Backend | [FastAPI](https://fastapi.tiangolo.com/) — serves the API and the frontend |
| Frontend | Plain HTML + CSS + JavaScript — no frameworks |
| Deployment | [Render](https://render.com/) |

---

## Project structure

```
ticket-categorizer/
│
├── main.py                  # FastAPI app — API routes and startup config
├── graph.py                 # LangGraph pipeline — the AI reasoning flow
├── schema.py                # Data models for input and output
├── requirements.txt         # Python dependencies
├── render.yaml              # Render deployment config
│
├── production_modules/      # Safety and reliability features
│   ├── pii_redaction.py     # Detects and removes personal data before sending to AI
│   ├── prompt_injection.py  # Blocks attempts to hijack the AI with malicious input
│   ├── prompt_versioning.py # Manages different versions of the AI prompt (v2, v3)
│   ├── structured_output.py # Forces the AI to return consistent, parseable results
│   ├── fallback_retry.py    # Retries failed API calls automatically
│   ├── cost_calculator.py   # Tracks token usage and cost per request
│   ├── validate_response.py # Validates the AI output before returning it
│   └── non_determinism.py   # Handles variability in AI responses
│
├── demo_ui/
│   └── index.html           # The web interface (single file, no framework)
│
└── tests/                   # Automated tests
```

---

## Running it locally

### Prerequisites
- Python 3.10 or higher
- A Gemini API key — get one free at [aistudio.google.com](https://aistudio.google.com/)

### Steps

**1. Clone the repo**
```bash
git clone https://github.com/Ramprasathls/ticket-categorizer.git
cd ticket-categorizer
```

**2. Create a virtual environment**
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Set up your environment variables**
```bash
cp .env.example .env
```
Open `.env` and add your Gemini API key:
```
GEMINI_API_KEY=your_key_here
PROMPT_VERSION=v3
DEFAULT_MODEL=gemini-flash-latest
LOG_COSTS=true
```

**5. Start the server**
```bash
uvicorn main:app --port 8000
```

**6. Open the app**

Go to `http://localhost:8000` in your browser. Use one of the sample tickets to test it out.

---

## A few things worth knowing

- **PII redaction runs before anything is sent to the AI.** If a ticket contains an email address or card number, it gets replaced with a placeholder before the Gemini API sees it.
- **Prompt injection is blocked.** If someone tries to override the AI's instructions through the ticket text, it gets caught and flagged.
- **Two prompt versions (v2 and v3) are available.** You can switch between them using the dropdown — v3 includes structured reasoning, v2 is a simpler classification prompt.
- **Cost tracking is built in.** Every response shows exactly how many tokens were used and what it cost.

---

## Built with

This project was built with the help of **[Antigravity](https://antigravity.dev/)** (Google DeepMind's AI coding assistant) and **Claude** (Anthropic). The architecture, production modules, and UI were developed through an iterative pair-programming workflow with these tools.

---

## License

MIT
