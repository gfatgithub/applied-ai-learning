# Applied AI Learning — Deep Dive: Large Language Models (LLMs)

## 📅 Duration: 1 Hour  
**Agenda with Timings**
- 0:00 – 0:05 → Welcome & Learning Objectives  
- 0:05 – 0:15 → What Are LLMs and How They're Built  
- 0:15 – 0:25 → How LLMs Actually Work (Step by Step)  
- 0:25 – 0:35 → Models in the Wild: Comparing Real LLMs  
- 0:35 – 0:45 → Hands-On: Working with LLMs (Live Demos)  
- 0:45 – 0:53 → Limitations, Risks & Safeguards  
- 0:53 – 0:57 → Glossary of Key Terms  
- 0:57 – 1:00 → References & Wrap-Up  

---

## 🎯 Learning Objectives
By the end of this session, you will be able to:
1. Explain how LLMs are trained and what makes them different from traditional software.  
2. Describe the step-by-step process of how an LLM generates text.  
3. Compare popular LLM models and know when to use which.  
4. Use LLMs effectively for real workplace tasks through live demos.  
5. Identify common risks (hallucinations, bias, data leakage) and apply safeguards.  

---

## 🧱 What Are LLMs and How They're Built (10 minutes)

### 1. Recap: What is an LLM? (1 minute)
A Large Language Model (LLM) is a computer program trained on massive amounts of text (books, websites, code) to predict what comes next in a sequence of words.  

**Key point:** LLMs don't "understand" language the way we do. They are very good at recognizing **patterns** in text and producing statistically likely responses.

---

### 2. How LLMs Are Trained (4 minutes)

Training an LLM is like teaching someone to write by having them read millions of books.

**The Three Phases of Training:**

~~~ascii
Phase 1: Pre-Training (The Heavy Lifting)
+---------------------------------------------+
| Feed BILLIONS of text documents              |
| (web pages, books, code, articles)           |
|                                              |
| Model learns patterns:                       |
|   "The capital of France is ___" → "Paris"   |
|   "def hello():" → likely Python code next   |
+---------------------------------------------+
         |
         v
Phase 2: Fine-Tuning (The Specialization)
+---------------------------------------------+
| Train on curated Q&A pairs                   |
| Example:                                     |
|   Q: "Summarize this email"                  |
|   A: [good summary example]                  |
|                                              |
| Makes model helpful and task-oriented        |
+---------------------------------------------+
         |
         v
Phase 3: RLHF (The Polish)
+---------------------------------------------+
| Reinforcement Learning from Human Feedback   |
|                                              |
| Humans rate AI responses:                    |
|   Response A: ★★★★★ (helpful, safe)          |
|   Response B: ★★☆☆☆ (vague, off-topic)      |
|                                              |
| Model learns to prefer better responses      |
+---------------------------------------------+
~~~

**Analogy:**  
- **Pre-training** = Reading every book in the library.  
- **Fine-tuning** = Taking a specialized course (e.g., "How to be a helpful assistant").  
- **RLHF** = Getting a mentor who says "this answer was great" or "try again."  

---

### 3. What "Large" Really Means (2 minutes)

"Large" refers to the number of **parameters** — think of them as tiny decision-making dials inside the model.

| Model Size | Parameters | Analogy |
|------------|-----------|---------|
| Small model | ~7 billion | A well-read college student |
| Medium model | ~70 billion | A senior expert |
| Large model | ~400+ billion | An entire research department |

**Why this matters for you:**  
- More parameters ≠ always better for your task.  
- Smaller models are **faster and cheaper**; larger models handle **complex reasoning** better.  
- Choosing the right size = saving money while getting good results.  

---

### 4. Open vs Closed Models (2 minutes)

| Feature | Open Models | Closed Models |
|---------|------------|---------------|
| Examples | LLaMA, Mistral, Gemma | GPT-4, Claude, Gemini |
| Can you see the code? | Yes | No |
| Can you run it yourself? | Yes (if you have hardware) | No (API only) |
| Cost model | Free to download; you pay for compute | Pay per API call |
| Data privacy | Your data stays on your servers | Data sent to provider |

**Analogy:**  
- **Open model** = Buying a car — you own it, maintain it, customize it.  
- **Closed model** = Taking a taxi — convenient, no maintenance, but you don't control the route.  

---

### 5. Wrap-Up Check (1 minute)
**Quick Poll:** "If you had to explain to a coworker how ChatGPT was built, what are the three phases?"  
(Answer: Pre-training → Fine-tuning → RLHF)

---

## ⚙️ How LLMs Actually Work — Step by Step (10 minutes)

### 1. The Full Pipeline (3 minutes)

When you type a message into ChatGPT (or any LLM), here's what happens:

~~~ascii
Step 1: TOKENIZATION
+--------------------------------------------------+
| Your input: "What are the top 3 sales trends?"   |
|                                                   |
| Broken into tokens:                               |
| ["What", " are", " the", " top", " 3",           |
|  " sales", " trends", "?"]                        |
+--------------------------------------------------+
              |
              v
Step 2: EMBEDDING (Converting to Numbers)
+--------------------------------------------------+
| Each token → a list of numbers (a vector)         |
|                                                   |
| "sales" → [0.23, -0.87, 0.45, 0.12, ...]         |
| "trends" → [0.19, -0.82, 0.51, 0.08, ...]        |
|                                                   |
| Similar words have similar numbers!               |
| "sales" is close to "revenue" in number-space     |
+--------------------------------------------------+
              |
              v
Step 3: ATTENTION (Finding Relationships)
+--------------------------------------------------+
| The model figures out which words relate to which  |
|                                                   |
| "top 3" ←→ "trends" (the model links these)       |
| "sales" ←→ "trends" (these are connected)          |
|                                                   |
| This is the "Transformer" magic                    |
+--------------------------------------------------+
              |
              v
Step 4: PREDICTION (One Token at a Time)
+--------------------------------------------------+
| Model predicts the NEXT most likely token          |
|                                                   |
| After generating "The top 3 sales trends are":    |
|   "1" → 72% likely                                 |
|   ":" → 15% likely                                 |
|   "as" → 8% likely                                 |
|                                                   |
| Picks "1" → then predicts next token → repeats     |
+--------------------------------------------------+
              |
              v
Step 5: OUTPUT
+--------------------------------------------------+
| Tokens reassembled into readable text              |
| "The top 3 sales trends are: 1. ..."              |
+--------------------------------------------------+
~~~

---

### 2. Temperature: The Creativity Dial (3 minutes)

**Temperature** controls how "creative" vs. "predictable" the model is.

~~~ascii
Temperature Scale:

Low (0.0 - 0.3)          Medium (0.4 - 0.7)        High (0.8 - 1.5)
+-----------------+       +-----------------+       +-----------------+
| Very predictable|       | Balanced        |       | Very creative   |
| Always picks    |       | Mix of expected |       | Surprising,     |
| the #1 answer   |       | and fresh ideas |       | sometimes wild  |
+-----------------+       +-----------------+       +-----------------+

Best for:               Best for:                Best for:
- Data extraction       - General Q&A            - Brainstorming
- Code generation       - Email drafting          - Creative writing
- Factual queries       - Summarization           - Idea generation
~~~

**Live Demo with ChatGPT:**  
Ask the same question twice with different instructions:

~~~text
Prompt 1: "List 3 ways to improve team meetings. Be precise and factual."
Prompt 2: "List 3 wildly creative ways to improve team meetings. Think outside the box."
~~~

Compare the outputs. The first behaves like low temperature; the second like high temperature.

---

### 3. Tokens and Costs in Practice (2 minutes)

Every interaction has a cost measured in tokens:

| Task | Approx. Tokens | Approx. Cost (GPT-4o) |
|------|-----------------|----------------------|
| "What's 2+2?" | ~20 tokens | < $0.001 |
| Summarize a 1-page email | ~1,500 tokens | ~$0.008 |
| Analyze a 50-page report | ~75,000 tokens | ~$0.38 |
| Daily chatbot (100 queries) | ~200,000 tokens | ~$1.00 |

**Key insight:** Input tokens AND output tokens both cost money.  
**Action:** Always check: "Am I sending more data than I actually need?"

---

### 4. Mini Exercise (2 minutes)
**Think about your last work task that involved writing or reading text.**  
- Could an LLM help?  
- How many tokens would it roughly need? (Remember: ~750 words ≈ 1,000 tokens)  

Share with the person next to you.

---

## 🏷️ Models in the Wild: Comparing Real LLMs (10 minutes)

### 1. The Major Players (3 minutes)

| Model | Company | Best For | Context Window |
|-------|---------|----------|----------------|
| GPT-4o | OpenAI | General-purpose, coding, analysis | 128k tokens (~200 pages) |
| GPT-4o mini | OpenAI | Fast, cheap tasks | 128k tokens |
| Claude 3.5 Sonnet | Anthropic | Long documents, careful reasoning | 200k tokens (~300 pages) |
| Gemini 1.5 Pro | Google | Multimodal (text + images + video) | 1M tokens (~1,500 pages) |
| LLaMA 3 | Meta | Open-source, self-hosted | 8k–128k tokens |
| Mistral Large | Mistral | European, open-weight, multilingual | 32k tokens |

**Analogy:**  
Choosing an LLM is like choosing a vehicle:
- **GPT-4o** = Reliable SUV — good at everything.  
- **GPT-4o mini** = Compact car — cheap, fast, handles most daily needs.  
- **Claude** = Luxury sedan — smooth, great for long trips (big documents).  
- **Gemini** = Multi-tool truck — handles images, video, and text.  
- **LLaMA/Mistral** = DIY kit car — you build and own it, full control.  

---

### 2. How to Choose the Right Model (3 minutes)

Use this decision flowchart:

~~~ascii
Start: What's your task?
         |
    +----+----+
    |         |
 Simple?   Complex?
    |         |
    v         v
Use a small   Does it need
/cheap model  long context?
(GPT-4o mini,   |
 Mistral)    +--+--+
             |     |
           Yes?   No?
             |     |
             v     v
          Claude  GPT-4o
          or      or
          Gemini  GPT-4o
          (long   (general
          docs)   purpose)
             |
             v
     Is data privacy critical?
             |
        +----+----+
        |         |
       Yes?      No?
        |         |
        v         v
     Self-host   Use API
     (LLaMA,    (OpenAI,
      Mistral)   Anthropic)
~~~

---

### 3. Live Demo: Same Question, Different Models (3 minutes)

**Exercise:** Ask the same question to two different AI tools:
- ChatGPT (GPT-4o)  
- Google Gemini  

Prompt:  
~~~text
You are a business analyst. Summarize the main advantages and disadvantages 
of remote work in exactly 5 bullet points. Use simple language.
~~~

**What to observe:**  
- Are the bullet points different?  
- Which response feels more useful?  
- Which one followed instructions more precisely?  

**Key takeaway:** Different models have different "personalities" — test before you commit.

---

### 4. Quick Quiz (1 minute)
"If you need to analyze a 200-page contract and data privacy is critical, which model approach would you choose?"  
(Answer: Self-hosted open model like LLaMA, or Claude via API with a data processing agreement.)

---

## 🛠️ Hands-On: Working with LLMs (10 minutes)

### 1. Demo: Summarization (3 minutes)

**Scenario:** You received a 2-page status report and need a 3-sentence summary.

**Step-by-step in ChatGPT:**
1. Open ChatGPT.  
2. Paste this prompt:  
~~~text
System: You are a concise business writer.
Task: Summarize the following status report in exactly 3 sentences. 
Focus on: key accomplishments, blockers, and next steps.

[Paste report text here]
~~~
3. Review the output.  
4. If too long, add: *"Make it shorter — max 50 words."*  

**Lesson:** LLMs follow instructions better when you specify **format, length, and focus area**.

---

### 2. Demo: Data Extraction (3 minutes)

**Scenario:** You have a messy email thread and need to pull out action items.

**Prompt:**
~~~text
Extract all action items from the email below. 
Format as a numbered list with: 
- WHO is responsible 
- WHAT they need to do 
- WHEN it's due

Email:
---
Hi team, John please send the Q3 report by Friday. 
Sarah, can you update the dashboard before our Monday meeting? 
Also Mike, we need the vendor contracts reviewed by end of month.
Thanks, Boss
---
~~~

**Expected output:**
~~~text
1. John — Send Q3 report — Due: Friday
2. Sarah — Update dashboard — Due: Before Monday meeting
3. Mike — Review vendor contracts — Due: End of month
~~~

---

### 3. Demo: Draft Generation (2 minutes)

**Scenario:** Write a professional reply to a customer complaint.

~~~text
System: You are a customer service representative. Be empathetic, professional, 
and solution-oriented. Keep the reply under 100 words.

Customer message: "I've been waiting 3 weeks for my order and nobody has 
responded to my emails. This is unacceptable."

Write a reply.
~~~

**Key lesson:** The system prompt sets the **tone**; the user prompt sets the **task**.

---

### 4. Group Exercise (2 minutes)
Pick one of these real scenarios and write a prompt for it:
1. Summarize meeting notes into action items.  
2. Translate a technical paragraph into simple language.  
3. Generate 3 subject lines for a newsletter.  

Share your prompt with the group.

---

## ⚠️ Limitations, Risks & Safeguards (8 minutes)

### 1. Hallucinations (2 minutes)

**What it is:** The model confidently generates information that is **completely false**.

**Example:**  
- Prompt: *"Who wrote the book 'The Silicon Path' published in 2019?"*  
- Model might answer: *"'The Silicon Path' was written by Dr. James Chen, published by TechPress in 2019."*  
- → This book, author, and publisher **may not exist**. The model invented them.

**Why it happens:**  
The model predicts likely-sounding text. If it doesn't know the answer, it generates one anyway — because generating text is all it does.

**Safeguard:**  
- **Always verify** facts from AI, especially names, dates, statistics, and references.  
- Use the prompt: *"If you're not sure, say 'I don't know' instead of guessing."*  

---

### 2. Bias (2 minutes)

**What it is:** The model reflects biases present in its training data.

**Example:**  
- Prompt: *"Write a job description for a nurse."*  
- Model might default to *"she"* — reflecting a gender bias from training data.

**Safeguard:**  
- Review AI outputs for **gender, cultural, and professional biases**.  
- Add to your prompt: *"Use gender-neutral language."*  
- Never use raw AI output for hiring, legal, or policy documents without human review.

---

### 3. Data Leakage & Privacy (2 minutes)

**What it is:** Sensitive information you send to an LLM could be stored or used for training.

~~~ascii
RISK:
+---------------------+         +-------------------+
| You paste customer  | ------> | Cloud LLM API     |
| names, SSNs, or     |         | (data may be      |
| internal financials  |         |  logged/trained)  |
+---------------------+         +-------------------+

SAFE APPROACH:
+---------------------+         +-------------------+
| Remove PII first    | ------> | Cloud LLM API     |
| OR use a self-      |         | (only sees safe   |
| hosted model        |         |  data)            |
+---------------------+         +-------------------+
~~~

**Safeguards:**  
- **Never paste** passwords, SSNs, financial data, or customer PII into public LLMs.  
- Use **enterprise plans** with data processing agreements (DPA).  
- For sensitive data, use **self-hosted models** (LLaMA, Mistral).  

---

### 4. Over-Reliance (1 minute)

**Pitfall:** Trusting AI output without checking. An LLM is a **starting point**, not the final answer.

**Rule of thumb:**  
- AI Draft → Human Review → Final Output  
- Think of the LLM as a **fast intern** — helpful, but needs supervision.

---

### 5. Summary of Risks & Safeguards (1 minute)

| Risk | Safeguard |
|------|-----------|
| Hallucination | Always fact-check; ask model to flag uncertainty |
| Bias | Review for bias; use neutral language prompts |
| Data leakage | Never paste PII; use enterprise/self-hosted |
| Over-reliance | Always have human review before final use |

---

## 📖 Glossary of Key Terms

- **LLM (Large Language Model):** A program trained on text data to predict and generate language.  
- **Token:** A small chunk of text (~3–4 characters) that the model processes.  
- **Parameter:** A learnable value inside the model; more parameters = more capacity.  
- **Context Window:** The maximum number of tokens the model can "see" at once.  
- **Temperature:** A setting that controls how creative (high) or predictable (low) output is.  
- **Pre-training:** Phase 1 — model reads massive text data to learn language patterns.  
- **Fine-tuning:** Phase 2 — model is trained on specific tasks to be more helpful.  
- **RLHF:** Reinforcement Learning from Human Feedback — humans rate outputs to improve quality.  
- **Embedding:** Converting text into numbers (vectors) so the model can process relationships.  
- **Attention:** The mechanism that lets the model understand how words relate to each other.  
- **Hallucination:** When the model generates false information confidently.  
- **Open Model:** An LLM whose weights/code are publicly available (e.g., LLaMA, Mistral).  
- **Closed Model:** An LLM accessible only via API, code not shared (e.g., GPT-4, Claude).  

---

## 🔗 References

1. [OpenAI — GPT-4 Technical Report](https://openai.com/index/gpt-4-research/)  
2. [Anthropic — Claude Model Card](https://docs.anthropic.com/en/docs/about-claude/models)  
3. [Meta — LLaMA 3 Introduction](https://ai.meta.com/blog/meta-llama-3/)  
4. [Google — Gemini Technical Report](https://deepmind.google/technologies/gemini/)  
5. [Stanford CS324 — Large Language Models](https://stanford-cs324.github.io/winter2022/)  
6. [Hugging Face — Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard)  
7. [OpenAI — Tokenizer Tool](https://platform.openai.com/tokenizer)  

---

## ✅ Wrap-Up

- LLMs are trained in three phases: pre-training, fine-tuning, and RLHF.  
- They work by predicting the next token — one at a time — based on patterns learned from training data.  
- Different models fit different needs: consider cost, privacy, context size, and task complexity.  
- Always verify AI outputs, protect sensitive data, and keep a human in the loop.  

**Next Steps for You:**  
1. Try the summarization and data extraction demos from this session using ChatGPT or another LLM.  
2. Compare two different models on the same prompt and note the differences.  
3. Review one document you handle regularly — could an LLM help? Estimate the token count.  
4. Bookmark the [OpenAI Tokenizer](https://platform.openai.com/tokenizer) to check token counts before sending long inputs.  

