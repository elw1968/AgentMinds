# 🧠 Explanation of Our Agent

This document explains the reasoning process, planning style, memory usage, tool integrations, and known limitations of our Agentic AI App.

---

##  1. Reasoning Process

1. Receive User Input  
   - The user enters a news-related claim or question (e.g., *"Is it true that AI will replace 90% of jobs by 2030?"*).

2. Plan Tasks (Planner)  
   - The agent breaks down the verification process into ReAct-style subtasks:
     - Search for live evidence  
     - Analyze & summarize evidence  
     - Generate a final verdict + probability score

3. Execute Tasks (Executor)  
   - Step 1: Fetch Evidence  
     - Uses SerpAPI to gather the top 3 relevant web sources.  
   - Step 2: Reason with Gemini  
     - Passes the claim + collected evidence to Google Gemini API with a structured prompt.  
     - Gemini responds with:  
       - Reasoning (why it’s true/false/unverified)  
       - Verdict (True / False / Unverified)  
       - Confidence level (Low / Medium / High)  
       - Probability score (0–100%)  

4. Generate Final Response  
   - Combines Gemini’s output and displays:  
      Verdict (color-coded)  
      Probability score (progress bar)  
      Top 3 evidence sources with clickable links

5. Update Memory  
   - Stores the user’s claim + verdict for future reference.

---

##  2. Planning Style

- Pattern Used: ReAct-style Agentic Workflow  
  - Think → Search → Reason → Respond  
  - The planner organizes sub-tasks logically before execution.

- Why This Style?  
  - Ensures a clear chain-of-thought reasoning, making it explainable.

---

##  3. Memory Usage

- Type: Simple on-disk JSON memory (not a vector database for lightweight implementation).  
- Stored Data:
  - User claim  
  - Generated verdict & probability  
- Usage:
  - Displayed as a Recent Verification Timeline in the UI.

---

##  4. Tool Integration

| Tool / API | Role |
|------------|------|
| Google Gemini API | Core reasoning engine to determine truthfulness |
| SerpAPI | Retrieves live web evidence snippets |
| NewsAPI | Fetches the latest trending news (Real/Fake) |
| dotenv | Manages secure API key loading |

---

##  5. Known Limitations

1. Dependency on API Responses  
   - If SerpAPI or NewsAPI fails, Gemini may rely solely on reasoning, lowering accuracy.

2. No Deep Fact-Checking Database  
   - The agent doesn’t cross-check with authoritative fact-checking databases (e.g., PolitiFact).

3. Limited Memory  
   - Only stores a short-term history; no long-term learning.

4. Probability Approximation  
   - The probability score is derived heuristically from Gemini’s confidence, not from statistical modeling.

5. Language Bias  
   - Works best with English-language claims.

---

##  6. Why It Stands Out?

- Real-time fact-checking using live evidence  
- Human-readable reasoning (transparent)  
- Probability-based scoring for better trust  
- Latest fake news updates tab