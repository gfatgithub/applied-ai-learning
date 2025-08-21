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

## 🧠 Introduction to LLMs  

**Plain English Explanation**  
Large Language Models (LLMs) are computer programs trained on massive amounts of text. They predict the next word in a sentence, allowing them to generate human-like answers, summaries, or even code.  

**Analogy**  
Think of an LLM like an **auto-complete on steroids**: instead of just finishing your text messages, it can complete reports, troubleshoot logs, or answer technical questions.  

**Workplace Example**  
Engineers can use an LLM to summarize long system logs into a concise error report.  

**Mini Demo (ASCII)**  
~~~text
User: Summarize system error log
AI: "Disk I/O bottleneck detected at node 3. Recommend workload balancing."
~~~

**Pitfalls & Safeguards**  
- Pitfall: Treating AI output as always correct.  
- Safeguard: Validate results with source data.  

**Summary + Action Item**  
LLMs are powerful text assistants. Action: Try using them for summarizing logs or generating technical documentation.  

---

## 🤖 Agentic AI  

**Plain English Explanation**  
Agentic AI means giving AI the ability to take actions, not just answer questions. It combines memory, tools, planning, and execution.  

**Analogy**  
Imagine a **junior engineer with a task list**: instead of only answering your questions, they open tools, check data, and send reports back.  

**Workplace Example**  
An AI agent could automatically pull server metrics, compare them to thresholds, and alert the team via Slack.  

**Mini Demo (ASCII n8n Flow)**  
~~~ascii
[Trigger: Daily 9am] --> [Node: API call "Server Stats"]
       |--> [Node: Compare vs Thresholds]
       |--> [Node: Slack Message "Alerts"]
~~~

**Pitfalls & Safeguards**  
- Pitfall: Infinite loops if not scoped.  
- Safeguard: Define clear limits (timeouts, retries).  

**Summary + Action Item**  
Agentic AI enables automation beyond Q&A. Action: Explore n8n or workflow tools to test one small agent task.  

---

## 📝 Prompt Engineering  

**Plain English Explanation**  
Prompt engineering is the art of asking AI the right way. Better prompts = better answers.  

**Analogy**  
It’s like **writing a ticket for IT support**: vague requests get vague solutions, but clear requests get clear fixes.  

**Workplace Example**  
When debugging logs, instead of “What’s wrong?”, you could ask:  
“Summarize top 3 error types in this 200-line log.”  

**Mini Demo (Bad vs Great Prompt)**  
~~~text
Bad Prompt:
"Explain logs."

Great Prompt:
"Summarize top 3 repeated errors from this 200-line log, and suggest 1 fix for each."
~~~

**Exercise**  
Rewrite this bad prompt:  
“Write something about cloud.”  
→ Try making it specific, technical, and scoped.  

**Pitfalls & Safeguards**  
- Pitfall: Overloading prompts with too much at once.  
- Safeguard: Break tasks into smaller steps.  

**Summary + Action Item**  
Good prompts drive clarity. Action: Practice rewriting vague instructions into precise ones.  

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
