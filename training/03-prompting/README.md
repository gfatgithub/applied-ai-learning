# Applied AI Learning — Deep Dive: Prompt Engineering

## 📅 Duration: 1 Hour  
**Agenda with Timings**
- 0:00 – 0:05 → Welcome & Learning Objectives  
- 0:05 – 0:15 → Foundations: How Prompts Shape AI Output  
- 0:15 – 0:25 → The Prompt Engineering Toolkit (Techniques & Patterns)  
- 0:25 – 0:40 → Hands-On: From Bad to Great Prompts (Live Demos & Exercises)  
- 0:40 – 0:50 → Advanced Techniques: Chain-of-Thought, Few-Shot & Role Prompting  
- 0:50 – 0:55 → Pitfalls, Anti-Patterns & Safeguards  
- 0:55 – 0:58 → Glossary of Key Terms  
- 0:58 – 1:00 → References & Wrap-Up  

---

## 🎯 Learning Objectives
By the end of this session, you will be able to:
1. Explain why prompt structure directly impacts AI output quality.  
2. Apply the four-part prompt template (Role, Context, Task, Format) to any request.  
3. Use advanced techniques: chain-of-thought, few-shot learning, and role prompting.  
4. Transform vague, low-quality prompts into precise, high-quality ones.  
5. Avoid common prompt engineering mistakes and anti-patterns.  

---

## 📐 Foundations: How Prompts Shape AI Output (10 minutes)

### 1. What is Prompt Engineering? (2 minutes)

**Prompt engineering** is the skill of writing clear, structured instructions so that AI produces useful, accurate, and relevant responses.

**Key insight:** The AI doesn't read your mind. It reads your **words**. The better your words, the better its output.

**Analogy:**  
Ordering food at a restaurant:
- Vague: *"Give me something good."* → You might get anything.  
- Precise: *"I'd like a grilled chicken salad, no onions, dressing on the side."* → You get exactly what you want.  

Prompting works the same way.

---

### 2. System Prompts vs User Prompts (3 minutes)

Every AI interaction has two layers of instructions:

~~~ascii
+--------------------------------------------------+
|  SYSTEM PROMPT (The Job Description)              |
|                                                   |
|  Sets the AI's role, tone, rules, and boundaries  |
|  Example: "You are a professional business        |
|  analyst. Always be concise. Use bullet points."  |
+--------------------------------------------------+
              |
              v
+--------------------------------------------------+
|  USER PROMPT (The Task Request)                   |
|                                                   |
|  The actual question or task from you              |
|  Example: "Summarize the Q3 sales report in       |
|  5 bullet points focusing on revenue trends."     |
+--------------------------------------------------+
              |
              v
+--------------------------------------------------+
|  AI OUTPUT                                        |
|  (Shaped by BOTH system + user prompts)            |
+--------------------------------------------------+
~~~

**Analogy:**  
- **System prompt** = Hiring someone for a role ("You are a data analyst").  
- **User prompt** = Giving them a specific task ("Analyze this spreadsheet").  

**Live Demo:**

Prompt WITHOUT a system prompt:
~~~text
"Tell me about our quarterly results."
~~~
→ AI gives a generic, unfocused response.

Prompt WITH a system prompt:
~~~text
System: "You are a financial analyst at a mid-sized tech company. 
Be concise and data-driven. Always include numbers."

User: "Summarize our Q3 results based on this data: 
Revenue: $4.2M (up 12%), Costs: $3.1M (up 8%), 
New customers: 340, Churn: 5%."
~~~
→ AI gives a focused, structured, data-rich summary.

---

### 3. Why Structure Matters: The 4-Part Template (3 minutes)

A well-structured prompt has **four parts**:

~~~ascii
+--------------------------------------------------+
| 1. ROLE (Who is the AI?)                          |
|    "You are a senior HR consultant."               |
+--------------------------------------------------+
              |
              v
+--------------------------------------------------+
| 2. CONTEXT (What background does it need?)         |
|    "We are a 200-person tech company.              |
|     We just completed an employee survey."         |
+--------------------------------------------------+
              |
              v
+--------------------------------------------------+
| 3. TASK (What should it do?)                       |
|    "Analyze the top 3 concerns from the survey     |
|     results and suggest one action for each."      |
+--------------------------------------------------+
              |
              v
+--------------------------------------------------+
| 4. FORMAT (How should the output look?)            |
|    "Respond as a numbered list. Each item:         |
|     concern, explanation (1 sentence), action."    |
+--------------------------------------------------+
~~~

**Template you can copy-paste:**
~~~text
Role: You are a [role]. 
Context: [Background information]
Task: [What you want the AI to do]
Format: [How the output should look — bullet points, table, JSON, etc.]
~~~

---

### 4. Live Demo: Template in Action (2 minutes)

**Without template:**
~~~text
"Help with the employee survey."
~~~
→ Vague, rambling answer.

**With template:**
~~~text
Role: You are a senior HR analyst with 10 years of experience.
Context: Our company (200 employees, tech sector) just completed an annual 
employee satisfaction survey. Top issues: work-life balance (72% concerned), 
career growth (65% concerned), management communication (58% concerned).
Task: For each of the top 3 concerns, explain why it matters and recommend 
one specific, actionable improvement.
Format: Numbered list. Each item: Concern → Why it matters (1 sentence) → 
Recommended action (1 sentence).
~~~
→ Clear, actionable, structured response.

---

## 🧰 The Prompt Engineering Toolkit (10 minutes)

### 1. Technique: Specificity & Constraints (3 minutes)

The more specific you are, the better the output.

**Vague → Specific progression:**

| Level | Prompt | Quality |
|-------|--------|---------|
| 😐 Vague | "Write about marketing." | Unfocused, generic |
| 🙂 Better | "Write about email marketing strategies." | Somewhat focused |
| 😊 Good | "List 5 email marketing strategies for a B2B SaaS company." | Focused, actionable |
| 🎯 Great | "List 5 email marketing strategies for a B2B SaaS company targeting mid-market IT buyers. Include one real-world example per strategy. Format as a table with columns: Strategy, Description, Example." | Precise, structured, useful |

**Constraints you can add:**
- **Length:** "In 3 sentences" / "Under 100 words" / "Exactly 5 bullet points"  
- **Audience:** "Explain to a non-technical manager" / "Write for a developer"  
- **Tone:** "Professional" / "Casual" / "Empathetic"  
- **Format:** "As a table" / "In JSON" / "As bullet points"  
- **Exclusions:** "Do not include pricing" / "Skip the introduction"  

---

### 2. Technique: Output Format Control (3 minutes)

Telling the AI **how** to format its answer is just as important as telling it **what** to answer.

**Common format instructions:**

~~~text
"Respond as a markdown table with columns: Feature, Benefit, Risk."
~~~

~~~text
"Give me the answer in this exact JSON format:
{
  'summary': '...',
  'action_items': ['...', '...'],
  'risk_level': 'low/medium/high'
}"
~~~

~~~text
"Format your response as:
## Finding
[one sentence]
## Impact  
[one sentence]
## Recommendation
[one sentence]"
~~~

**Live Demo:**

Same question, different format requests:

~~~text
Prompt A: "What are the benefits of remote work?"
→ AI writes paragraphs (hard to scan)

Prompt B: "List the top 5 benefits of remote work as a numbered list. 
Each item: benefit name in bold, then one sentence explanation."
→ Clean, scannable, useful
~~~

---

### 3. Technique: Negative Instructions (2 minutes)

Sometimes telling the AI what **NOT** to do is just as important.

**Examples:**
~~~text
"Summarize this report. Do NOT include the methodology section."
"Write a product description. Do NOT use buzzwords like 'revolutionary' or 'game-changing'."
"Explain machine learning to a 10-year-old. Do NOT use technical jargon."
~~~

**Why this works:**  
LLMs tend to include everything by default. Negative instructions act as **guardrails** that narrow the output.

---

### 4. Exercise: Upgrade These Prompts (2 minutes)

Take these vague prompts and rewrite them using the 4-part template:

**Prompt 1:** *"Tell me about cloud computing."*  
→ Your improved version: ___

**Prompt 2:** *"Write an email."*  
→ Your improved version: ___

**Prompt 3:** *"Analyze this data."*  
→ Your improved version: ___

**Example answer for Prompt 1:**
~~~text
Role: You are a cloud computing instructor for non-technical professionals.
Context: The audience has never used cloud services and works in a traditional 
office environment.
Task: Explain what cloud computing is and list 3 benefits most relevant 
to office workers.
Format: Start with a one-sentence definition, then a numbered list. 
Keep each benefit under 20 words.
~~~

---

## 🛠️ Hands-On: From Bad to Great Prompts (15 minutes)

### 1. Live Demo: Log Analysis — Bad vs Good (4 minutes)

**Bad Prompt:**
~~~text
"Explain logs."
~~~

**Likely AI response:** A generic explanation of what logs are. Not useful.

---

**Good Prompt:**
~~~text
Role: You are a senior systems administrator.
Context: I have a 200-line server log from a Linux web server. 
The server has been experiencing intermittent 502 errors for the past 3 hours.
Task: Identify the top 3 most repeated errors, explain what each one means 
in simple terms, and suggest one fix for each.
Format: Numbered list. Each item: Error message → Explanation (1 sentence) → 
Suggested fix (1 sentence).

[Paste log excerpt here]
~~~

**Expected output:**
~~~text
1. 502 Bad Gateway (repeated 45 times) → The web server couldn't get a 
   response from the backend. → Fix: Restart the backend service and check 
   if it's running out of memory.
2. Connection timeout to database (30 times) → The app can't connect to 
   the database within the time limit. → Fix: Increase connection pool 
   size or check database server load.
3. Disk space warning (18 times) → The server's storage is nearly full. 
   → Fix: Clear old logs and temporary files.
~~~

---

### 2. Live Demo: Email Drafting — Iterative Improvement (4 minutes)

**Round 1 — Too vague:**
~~~text
"Write an email about the project delay."
~~~
→ Generic and doesn't match your situation.

**Round 2 — Better, with context:**
~~~text
"Write an email to the client explaining that the website redesign project 
is delayed by 2 weeks due to unexpected technical issues."
~~~
→ Good content, but tone might be wrong.

**Round 3 — Great, with role + format:**
~~~text
Role: You are a project manager writing to a valued long-term client.
Context: The website redesign project (originally due April 30) is delayed 
by 2 weeks. Cause: the payment integration required additional security testing. 
The team is working overtime to minimize further delays.
Task: Write an apologetic but confident email that explains the delay, 
reassures the client, and provides the new timeline.
Format: Professional email structure — greeting, 2-3 short paragraphs, 
sign-off. Keep under 150 words. Tone: empathetic but confident.
~~~

**Key lesson:** Prompt engineering is often **iterative**. Start rough, refine, improve.

---

### 3. Live Demo: Data Transformation (3 minutes)

**Scenario:** You have messy data and need it cleaned up.

~~~text
Role: You are a data analyst.
Context: Below is a list of customer names and emails extracted from 
a messy spreadsheet. Some entries have typos or formatting issues.
Task: Clean and standardize the data.
Format: Return as a markdown table with columns: Name (First Last, 
title case), Email (lowercase, validated format).

Raw data:
john DOE, JOHNDOE@EMAIL.COM
jane smith, jane.smith@company
BOB WILSON, bob.wilson@work.com
alice JOHNSON, Alice.Johnson@mail.com
~~~

**Expected output:**

| Name | Email |
|------|-------|
| John Doe | johndoe@email.com |
| Jane Smith | jane.smith@company (⚠️ missing domain extension) |
| Bob Wilson | bob.wilson@work.com |
| Alice Johnson | alice.johnson@mail.com |

---

### 4. Group Exercise: Prompt Challenge (4 minutes)

**Challenge:** Pick one scenario below and write the best prompt you can in 2 minutes. Then share with the group.

**Scenario A:** You need to translate a technical error message into language a non-technical manager can understand.

**Scenario B:** You need to generate interview questions for a junior data analyst position.

**Scenario C:** You need to compare two vendor proposals and recommend one.

**After sharing:** The group votes on which prompt would produce the best output. Discuss what made the winning prompt effective.

---

## 🧠 Advanced Techniques (10 minutes)

### 1. Chain-of-Thought Prompting (3 minutes)

**What it is:** Asking the AI to show its reasoning step by step before giving the final answer.

**Why it works:**  
When the AI "thinks out loud," it makes fewer errors — especially on math, logic, and multi-step problems.

**Without chain-of-thought:**
~~~text
"If a store has 15 apples and sells 40% of them, then receives a 
shipment of 20 more, how many apples does it have?"

AI answer: "27 apples" (might be wrong — no visible reasoning)
~~~

**With chain-of-thought:**
~~~text
"If a store has 15 apples and sells 40% of them, then receives a 
shipment of 20 more, how many apples does it have?

Think step by step before giving the final answer."

AI answer: 
"Step 1: Start with 15 apples.
 Step 2: 40% of 15 = 6 apples sold.
 Step 3: 15 - 6 = 9 apples remaining.
 Step 4: 9 + 20 = 29 apples.
 Final answer: 29 apples." ✓
~~~

**When to use:** Math, logic puzzles, multi-step analysis, debugging, comparisons.

**Magic phrases to add to your prompts:**
- *"Think step by step."*  
- *"Show your reasoning before answering."*  
- *"Walk me through your logic."*  

---

### 2. Few-Shot Prompting (3 minutes)

**What it is:** Giving the AI **examples** of what you want before asking it to do the task.

**Analogy:** Instead of explaining the rules of a game, you show someone a few rounds being played.

**Without examples (Zero-Shot):**
~~~text
"Classify the following customer message as Positive, Negative, or Neutral:
'The product arrived on time but the packaging was damaged.'"

→ AI might say "Negative" (not quite right — it's mixed)
~~~

**With examples (Few-Shot):**
~~~text
"Classify customer messages as Positive, Negative, or Neutral.

Examples:
- 'Great product, love it!' → Positive
- 'Terrible service, never again.' → Negative
- 'The product is okay, nothing special.' → Neutral
- 'Fast shipping but item was scratched.' → Mixed

Now classify:
'The product arrived on time but the packaging was damaged.'"

→ AI says: "Mixed" ✓ (learned the pattern from examples)
~~~

**When to use:** Classification, formatting, style matching, translation, any task where showing is easier than explaining.

**Template:**
~~~text
[Instruction]
Here are some examples:
- Input: [example 1] → Output: [result 1]
- Input: [example 2] → Output: [result 2]
- Input: [example 3] → Output: [result 3]

Now do the same for:
- Input: [your actual input]
~~~

---

### 3. Role Prompting (2 minutes)

**What it is:** Assigning the AI a specific expert persona so its answers are shaped by that expertise.

**Different roles, different answers:**

Same question: *"How should we handle the project delay?"*

| Role | Response Style |
|------|---------------|
| "You are a project manager" | Timeline, milestones, resource reallocation |
| "You are a client relationship manager" | Client communication, expectation setting |
| "You are a risk analyst" | Impact assessment, contingency planning |
| "You are an executive" | High-level summary, business impact |

**Live Demo:**
~~~text
Prompt 1: "You are a cybersecurity expert. Explain why password123 is a bad password."
→ Technical, detailed response about brute force and dictionary attacks.

Prompt 2: "You are a kindergarten teacher. Explain why password123 is a bad password."
→ Simple, fun explanation: "It's like leaving your diary on the playground with a sign that says 'READ ME'!"
~~~

---

### 4. Quick Technique Summary (2 minutes)

| Technique | When to Use | Magic Words |
|-----------|------------|-------------|
| **4-Part Template** | Every prompt | Role + Context + Task + Format |
| **Chain-of-Thought** | Math, logic, multi-step | "Think step by step" |
| **Few-Shot** | Classification, formatting | "Here are some examples:" |
| **Role Prompting** | Specialized expertise | "You are a [expert role]" |
| **Negative Instructions** | Avoiding unwanted content | "Do NOT include..." |
| **Specificity** | Always | Length, audience, tone, format constraints |

---

## 🚫 Pitfalls, Anti-Patterns & Safeguards (5 minutes)

### 1. The "Kitchen Sink" Anti-Pattern (1 minute)

**Pitfall:** Cramming too many unrelated tasks into one prompt.

**Bad:**
~~~text
"Summarize this report, translate it to Spanish, write a Slack message about it, 
and also create a budget forecast for next quarter."
~~~

**Why it fails:** The AI gets confused, prioritizes some tasks, and does all of them poorly.

**Fix:** One prompt per task, or clearly separate tasks:
~~~text
"Do the following tasks in order:
1. Summarize the report in 3 sentences.
2. Translate the summary to Spanish.
3. Write a Slack message (under 50 words) sharing the summary."
~~~

---

### 2. The "Mind Reader" Anti-Pattern (1 minute)

**Pitfall:** Assuming the AI knows your context without telling it.

**Bad:**
~~~text
"Update the thing for the meeting tomorrow."
~~~

**What the AI doesn't know:** What "thing"? Which meeting? What format? For whom?

**Fix:** Always provide **who, what, when, why, and how:**
~~~text
"Update the project status slide (PowerPoint format) for tomorrow's 
client meeting with Acme Corp. Focus on milestone progress and 
next steps. Keep it to 3 bullet points."
~~~

---

### 3. The "Blind Trust" Anti-Pattern (1 minute)

**Pitfall:** Using AI output without verifying.

**Examples of risky blind trust:**
- Using AI-generated statistics in a presentation without checking sources.  
- Sending an AI-drafted email without reading it.  
- Copying AI-generated code into production without testing.  

**Safeguard:** Always follow the **Draft → Review → Use** workflow:
~~~ascii
[AI generates output]
         |
         v
[YOU review for accuracy, tone, and relevance]
         |
         v
[Fix any issues]
         |
         v
[USE the final version]
~~~

---

### 4. The "One-Shot Wonder" Anti-Pattern (1 minute)

**Pitfall:** Expecting the first prompt to produce a perfect result.

**Reality:** Prompt engineering is **iterative**. Plan for 2–3 rounds of refinement.

**Workflow:**
~~~ascii
Round 1: Write your best prompt → Get result
         |
         v
Round 2: "Make it shorter" / "Add more detail" / "Change the tone"
         |
         v
Round 3: "Perfect — use this version."
~~~

**Tip:** Save your best prompts as **templates** for future reuse.

---

### 5. Summary of Anti-Patterns (1 minute)

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| Kitchen Sink | Too many tasks in one prompt | Split into separate prompts |
| Mind Reader | Missing context | Always provide who/what/when/why/how |
| Blind Trust | No verification | Draft → Review → Use |
| One-Shot Wonder | Expecting perfection first try | Iterate and refine |

---

## 📖 Glossary of Key Terms

- **Prompt Engineering:** The practice of designing structured instructions to get better AI outputs.  
- **System Prompt:** Instructions that set the AI's role, tone, and rules (like a job description).  
- **User Prompt:** The specific task or question from the user (like a work request).  
- **Chain-of-Thought (CoT):** A technique where the AI explains its reasoning step by step.  
- **Few-Shot Prompting:** Providing examples in the prompt so the AI learns the expected pattern.  
- **Zero-Shot Prompting:** Asking the AI to perform a task without any examples.  
- **Role Prompting:** Assigning the AI a specific expert persona to shape its responses.  
- **Temperature:** A setting controlling creativity (low = predictable, high = creative).  
- **Negative Instructions:** Telling the AI what NOT to do to narrow the output.  
- **Prompt Template:** A reusable structure (Role + Context + Task + Format) for consistent results.  
- **Iterative Prompting:** Refining a prompt across multiple rounds for better output.  
- **Hallucination:** When AI generates confident but incorrect information.  

---

## 🔗 References

1. [OpenAI — Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)  
2. [Anthropic — Prompt Engineering Documentation](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)  
3. [Google — Prompting Strategies for Gemini](https://ai.google.dev/gemini-api/docs/prompting-strategies)  
4. [Learn Prompting — Free Course](https://learnprompting.org/)  
5. [Chain-of-Thought Prompting Paper (Wei et al.)](https://arxiv.org/abs/2201.11903)  
6. [Few-Shot Learning with Language Models (Brown et al.)](https://arxiv.org/abs/2005.14165)  
7. [Prompt Engineering Institute](https://www.promptengineering.org/)  

---

## ✅ Wrap-Up

- The quality of your AI output depends directly on the quality of your prompt.  
- Use the **4-part template** (Role + Context + Task + Format) for every important prompt.  
- Advanced techniques like **chain-of-thought** and **few-shot** dramatically improve results.  
- Prompt engineering is **iterative** — plan for 2–3 refinement rounds.  
- Avoid anti-patterns: don't overload prompts, don't skip context, always verify output.  

**Next Steps for You:**  
1. Pick one real task you do this week and write a prompt using the 4-part template.  
2. Try chain-of-thought: add "Think step by step" to your next complex question.  
3. Save your 3 best prompts as reusable templates.  
4. Practice the "Bad → Good" exercise: take a vague prompt and improve it using the techniques from this session.  
