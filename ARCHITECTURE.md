# 🏗 Architecture Overview – Fake News Detection Agent 2.0

This document describes the high-level architecture of our Agentic AI App built for the hackathon.

---

##  Components

### 1. User Interface (Frontend)
- Streamlit Web App
  - Simple & responsive interface
  - Input: User enters a news claim/question
  - Output: 
    - 🏆 Verdict (True / False / Unverified)
    - 📊 Probability Score (0–100%)
    - 🔗 Top 3 Evidence Sources (clickable links)
    - 📰 Latest Fake News Tab
    - 📜 Recent Verification Timeline (Memory display)

---

### 2. Agent Core (Reasoning Engine)

#### 🧠 Planner (planner.py)
- Breaks the claim into ReAct-style subtasks:
  1. Search for related evidence (via SerpAPI/NewsAPI)
  2. Analyze evidence using Gemini API
  3. Generate reasoning & probability-based verdict

#### ⚙️ Executor (executor.py)
- Handles the logic of:
  - Fetching live evidence (search_web, fetch_latest_news)
  - Prompting Google Gemini API with structured instructions
  - Calculating & returning probability + reasoning + verdict

#### 🗂 Memory (memory.py)
- Stores recent verifications as JSON (on-disk memory)
- Displayed in the Streamlit app as "Recent Verification Timeline"

---

### 3. Tools / APIs Integrated

| Tool/API | Purpose |
|----------|---------|
| Google Gemini API | Core reasoning & generating final verdict |
| SerpAPI | Fetches top live web evidence (titles, snippets, links) |
| NewsAPI | Fetches latest trending news (Fake/Real) for Tab 2 |
| dotenv | Securely loads API keys from .env |

---

### 4. Observability & Error Handling
- Logging:
  - Every reasoning step (planned tasks displayed in the UI)
  - Stores input, verdict, and confidence in memory
- Error Handling:
  - If no evidence is found → returns "UNVERIFIED"
  - Retries Gemini call if API error occurs

---

##  Why This Architecture?
- Modular design → Easy to upgrade (Planner, Executor, Memory separated)
- Real-time evidence → More reliable fact-checking
- Agentic workflow (Plan → Execute → Memory → Output)
- Interactive UI → User-friendly verification process