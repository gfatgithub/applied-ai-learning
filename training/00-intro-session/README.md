# Applied AI Learning — Introductory Session

## 📅 Duration: 1 Hour  
**Agenda with Timings**
- 0:00 – 0:05 → Welcome & Learning Objectives  
- 0:05 – 0:15 → Introduction to LLMs  
- 0:15 – 0:25 → Agentic AI (with n8n sample)  
- 0:25 – 0:40 → Prompt Engineering (bad vs great, with exercises)  
- 0:40 – 0:50 → Test-Time Compute (with practical example)  
- 0:50 – 0:55 → Glossary of AI Terms  
- 0:55 – 1:00 → References & Wrap-up  

---

## 🎯 Learning Objectives
By the end of this session, you will be able to:
1. Explain what Large Language Models (LLMs) are in simple terms.  
2. Describe what Agentic AI is and how tools like n8n can be used.  
3. Apply prompt engineering techniques to get better results from AI.  
4. Understand test-time compute and its workplace impact.  
5. Recognize common pitfalls and safeguards in AI use.  

---

## 🧠 Introduction to LLMs (15 minutes)

### 1. What is an LLM? (2 minutes)
Large Language Models (LLMs) are programs trained on massive text data.  
They work by predicting the **next token** (a chunk of text) based on what came before.  
They are not databases; they are **probability** engines.  

**Analogy:** An LLM is like a **supercharged autocomplete**.  
Where your phone finishes "How are …" with "you?", an LLM can draft full reports, troubleshoot logs, or generate code.

---

### 2. Tokens: The LEGO Bricks of Language (5 minutes)
LLMs don’t process whole words directly. They use **tokens** — small pieces of text.  
- A token ≈ 3–4 characters in English on average.  
- "Automation" → might split into: ["Auto", "mation"]  

**Mini Demo: Tokenization**
~~~text
Input: "The quick brown fox jumps"
Tokenized: ["The", " quick", " brown", " fox", " jumps"]
~~~

**Live Demo with ChatGPT**
Ask the model:  
~~~text
Split the sentence "The quick brown fox jumps over the lazy dog." into tokens.
Also tell me how many tokens are in it.
~~~

Expected output (approximate):  
- Tokens: ["The", " quick", " brown", " fox", " jumps", " over", " the", " lazy", " dog", "."]  
- Count: 10 tokens  

This demonstrates that LLMs “see” **chunks**, not whole sentences.

**Why tokens matter:**
- **Costs** → Every token is like a **text message fee**.  
  - Example: GPT-4o costs ~$0.005 per 1,000 tokens (input + output).  
  - 1,000 tokens ≈ 750 words (~1 page).  
  - So, a 10-page input + 2-page output = ~12,000 tokens = ~$0.06 per run.  
  - At scale, costs accumulate quickly.  
- **Limits** → Tokens define the **maximum conversation size**.  
  - Think of tokens as **lines on a whiteboard**.  
  - A 4k-token model = ~6 pages.  
  - A 32k-token model = ~50 pages.  
  - A 200k-token model = ~300 pages.  

**Analogy:** Tokens are **LEGO bricks** of language.  
The LLM doesn’t see paragraphs — it builds text piece by piece.

---

### 3. How LLMs Work: ASCII Pipeline (2 minutes)
~~~ascii
User Input
   |
   v
+-------------------+
|   Tokenizer       | -> Splits text into tokens
+-------------------+
   |
   v
+-------------------+
|   LLM Engine      | -> Predicts next token
|   (probabilities) |
+-------------------+
   |
   v
+-------------------+
|   Detokenizer     | -> Reassembles tokens into text
+-------------------+
   |
   v
AI Output
~~~

---

### 4. Context Window: The Whiteboard (3 minutes)
Each LLM has a **context window** — the maximum tokens it can “see” at once.  
- Small models: ~4k tokens (~6 pages).  
- Medium models: 32k (~50 pages).  
- Largest models: 200k (~300 pages).  

**Analogy:** The context window = **whiteboard size**.  
- Small whiteboard → you erase often.  
- Big whiteboard → you can keep more notes.  

**Live Demo: Overload Context**
1. Paste a huge block of text (e.g., many pages of repeated words).  
2. Ask:  
   ~~~text
   At the very beginning of this text, I wrote a secret phrase. What was it?
   ~~~  
3. If the text exceeds the context window, the model forgets the start.  
→ Shows the model doesn’t truly “remember”; it only works with what fits on the board.

---

### 5. Context vs Memory (2 minutes)
- **Context** = temporary, session-based info in the whiteboard.  
- **Memory** = long-term recall across sessions (requires external DB).  

**ASCII Visual**
~~~ascii
[ Context Window = Whiteboard ]
+--------------------------------+
| Things written now             |
| (recent messages, data)        |
+--------------------------------+
   |
   v
Erased once full

[ Memory = Filing Cabinet ]
+--------------------------------+
| Stored across sessions         |
| Requires external DB/system    |
+--------------------------------+
~~~

**Mini Demo: Session Recall**
1. Say:  
   ~~~text
   Remember this phrase: ZEBRA-42
   ~~~  
2. Ask in the same chat:  
   ~~~text
   What phrase did I ask you to remember?
   ~~~  
   → Works (short-term context).  
3. Ask in a **new chat**:  
   ~~~text
   What phrase did I ask you to remember in the other chat?
   ~~~  
   → No recall unless explicit memory features are enabled.  

---

### 6. Capabilities & Limitations (2 minutes)
**Capabilities:** Summarization, Q&A, translation, coding, report generation.  
**Limitations:**  
- **Hallucination**: Makes up facts with confidence.  
- **Bias**: Reflects flaws in training data.  
- **Context bound**: Can’t exceed window size.  
- **No innate long-term memory**: Needs scaffolding for persistence.  

---

### 7. Wrap-Up Exercise (2 minutes)
Ask the group:  
- *“If an LLM can only see 32k tokens (~50 pages), how would you process a 500-page technical manual?”*  
(Hints: split into chunks, summarize each, use retrieval augmentation.)  

---

### ✅ Summary + Action Items
- LLMs process **tokens**, not whole words.  
- **Costs scale with tokens** — every input and output has a price.  
- **Context window** = whiteboard size → finite.  
- **Memory** = external filing cabinet; not built in.  
- Demos show both **tokenization** and **context limits**.  
- Action: Always **chunk large data, track token usage, and validate outputs**.

---

## 🤖 Agentic AI (10 minutes)

### 1. What is Agentic AI? (2 minutes)
Agentic AI is AI that can **act with autonomy**. Instead of only answering questions, it can:  
- **Plan**: Decide next steps toward a goal.  
- **Use tools**: Call calculators, APIs, or databases.  
- **Leverage memory**: Recall past inputs or conversations.  
- **Execute tasks**: Chain steps together automatically.  

**Definition:**  
Agentic AI combines **reasoning, memory, and tool use** so the AI can **learn from context, plan actions, and adapt** — like a junior engineer who doesn’t just reply but takes initiative.

---

### 2. Analogy (1 minute)  
Think of giving a new team member a problem:  
- A **regular AI** is like someone who answers your question and stops.  
- An **agentic AI** is like someone who answers, then **uses a calculator, checks past notes, and sends you a full report**.

---

### 3. Workplace Example (2 minutes)  
- IT: AI checks logs, runs diagnostics, and alerts teams.  
- Business: AI drafts reports, queries a database, and emails results.  

---

### 4. Demo Workflows in n8n (3 minutes)  

**a) AI Agent with Tool (Calculator)**  
~~~ascii
[Manual Trigger] → [Edit Fields] → [AI Agent]
       |-- Chat Model: OpenAI
       |-- Tool: Calculator
~~~
💡 Demo: Ask "What’s 25 × 17?" → The agent **uses the calculator** → Returns result.  

**b) ChatGPT with and without Memory**  
~~~ascii
[When message received] → [ChatGPT - No Memory]  
                         → [ChatGPT - With Memory + DB]
~~~
💡 Demo:  
- *No Memory*: Forgets prior chats once session ends.  
- *With Memory*: Recalls earlier inputs (via external DB).  

This shows **context = short-term scratchpad**, while **memory = long-term filing cabinet**.  

---

### 5. Pitfalls & Safeguards (1 minute)  
- **Pitfalls**:  
  - Infinite loops if not scoped.  
  - Wrong tool usage if instructions are unclear.  
  - Memory can introduce errors if data is outdated.  
- **Safeguards**:  
  - Define boundaries (timeouts, retries).  
  - Validate outputs before action.  
  - Set rules for memory updates.  

---

### 6. Summary + Action Item (1 minute)  
Agentic AI enables **autonomy, memory, and tool use** — letting AI act, not just respond.  
**Action:** Try an n8n workflow where the AI uses a tool or remembers past chats, starting with a small, safe task.  

---

## 📝 Prompt Engineering (15 minutes)

### 1. What is Prompt Engineering? (2 minutes)  
Prompt engineering is the practice of designing clear, structured instructions so AI systems generate useful, accurate, and relevant responses. The way you phrase a request directly shapes the quality of the answer.  

---

### 2. System vs User Prompts (3 minutes)  
- **System Prompt**: Establishes the AI’s role, style, or boundaries.  
  - Example: *"You are a helpful IT support assistant. Always respond concisely and professionally."*  
- **User Prompt**: Represents the actual task or question from the user.  
  - Example: *"Summarize the top 3 errors in this server log and suggest fixes."*  

**Analogy**  
Think of the system prompt as the **job description**, and the user prompt as the **task request**.  

---

### 3. Structured Prompt Template (3 minutes)  
A good structured prompt often contains four parts:  

~~~text
[System Prompt]  
You are an expert [role]. Follow the instructions carefully.  

[Context]  
Here is the background information you need to consider.  

[Task / User Request]  
Do X, Y, and Z with this input.  

[Output Format]  
Respond in JSON with fields "summary" and "recommendations".
~~~

---

### 4. Demo: Bad vs Good Prompt with ChatGPT (4 minutes)  

**Bad Prompt Example**  
~~~text
"Explain logs."
~~~

**ChatGPT Result (likely vague):**  
*"These logs contain information about errors and processes that occurred on the system."*  

---

**Good Prompt Example**  
~~~text
System: "You are a log analysis assistant. Always be concise."  
User: "Summarize the top 3 repeated errors from this 200-line server log and suggest one fix for each."  
~~~

**ChatGPT Result (clear and actionable):**  
- Error 1: Database timeout (repeated 45 times) → Fix: Increase connection pool size.  
- Error 2: Disk space warning (30 times) → Fix: Add monitoring + cleanup scripts.  
- Error 3: Authentication failure (18 times) → Fix: Reset expired API keys.  

---

### 5. Exercise (2 minutes)  
Take this vague prompt:  
*"Write something about cloud."*  
Rewrite it into a precise, scoped request with context and desired output format.  

---

### 6. Pitfalls & Safeguards (1 minute)  
- **Pitfall**: Overloading prompts with multiple unrelated tasks.  
- **Safeguard**: Break complex jobs into smaller steps.  

---

### 7. Summary + Action Item (1 minute)  
Prompts shape AI output quality.  
**Action:** Always pair a **system prompt** for consistency with a **clear user prompt** for the actual task, and structure them with context + output requirements.  


---

## ⚡ Test-Time Compute  

**Plain English Explanation**  
Test-time compute is how much computing power is used **when running the AI model** (not when training it). More compute often means better answers but also higher costs.  

**Analogy**  
Like choosing between a **quick sketch vs. a detailed blueprint**: both are useful, but one takes more time and resources.  

**Workplace Example**  
Running a “quick” AI query to summarize 10 log lines costs less compute than asking it to analyze 10,000 logs for anomalies.  

**Mini Demo (ASCII)**  
~~~ascii
Low Compute (fast):
Input: 10 log lines → AI → Quick summary

High Compute (slow but detailed):
Input: 10,000 log lines → AI → Full anomaly report
~~~

**Pitfalls & Safeguards**  
- Pitfall: Using high compute for trivial tasks.  
- Safeguard: Match compute level to task complexity.  

**Summary + Action Item**  
Balance accuracy vs cost. Action: Decide when you need quick vs detailed answers.  

---

## 📖 Glossary of AI Terms  

- **LLM (Large Language Model):** A system trained to predict text.  
- **Agentic AI:** AI that can act via tools and workflows.  
- **Prompt Engineering:** Crafting input instructions for better AI output.  
- **Test-Time Compute:** The compute resources used when running AI.  
- **Hallucination:** When AI confidently gives wrong information.  

---

## 🔗 References  

1. [OpenAI – How LLMs Work](https://platform.openai.com/docs)  
2. [n8n Documentation](https://docs.n8n.io)  
3. [Stanford – Introduction to LLMs](https://web.stanford.edu/~jurafsky/slp3/)  
4. [Anthropic on Test-Time Compute](https://www.anthropic.com)  

---

## ✅ Wrap-Up  

- You learned the basics of LLMs, Agentic AI, prompt engineering, and test-time compute.  
- Remember: start small, validate outputs, and scale responsibly.  

**Next Steps for You:**  
1. Try one small AI task in your work this week.  
2. Practice rewriting a vague prompt into a precise one.  
3. Explore n8n to see how automation flows can work.  

---
