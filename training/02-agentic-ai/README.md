# Applied AI Learning — Deep Dive: Agentic AI

## 📅 Duration: 1 Hour  
**Agenda with Timings**
- 0:00 – 0:05 → Welcome & Learning Objectives  
- 0:05 – 0:15 → What is Agentic AI? (Concepts & Architecture)  
- 0:15 – 0:25 → The Building Blocks: Tools, Memory & Planning  
- 0:25 – 0:40 → Hands-On: Agentic Workflows with n8n  
- 0:40 – 0:50 → Real Workplace Use Cases & Patterns  
- 0:50 – 0:55 → Risks, Pitfalls & Safeguards  
- 0:55 – 0:58 → Glossary of Key Terms  
- 0:58 – 1:00 → References & Wrap-Up  

---

## 🎯 Learning Objectives
By the end of this session, you will be able to:
1. Define Agentic AI and explain how it differs from basic chatbots.  
2. Describe the four building blocks of an AI agent: reasoning, tools, memory, and planning.  
3. Build and understand simple agentic workflows in n8n.  
4. Identify real workplace tasks where agentic AI adds value.  
5. Recognize risks of autonomous AI systems and apply safeguards.  

---

## 🤖 What is Agentic AI? (10 minutes)

### 1. The Big Idea (2 minutes)

Most AI tools today are **reactive** — you ask a question, you get an answer, and the AI stops.  
**Agentic AI** goes further. It can **think, plan, act, and learn** — completing multi-step tasks on its own.

~~~ascii
Regular AI (Reactive):
+----------+     +---------+     +----------+
| You ask  | --> | AI      | --> | One      |
| a question|    | answers |     | response |
+----------+     +---------+     +----------+
   DONE. AI stops here.

Agentic AI (Autonomous):
+----------+     +---------+     +-----------+     +----------+     +--------+
| You give | --> | AI      | --> | AI uses   | --> | AI checks| --> | AI     |
| a goal   |     | plans   |     | tools     |     | results  |     | adapts |
+----------+     +---------+     +-----------+     +----------+     +--------+
                      ^                                  |
                      |__________________________________| 
                            (loops until goal is met)
~~~

---

### 2. Analogy: The Intern vs The Project Manager (2 minutes)

Think of it this way:

- **Regular AI** = An intern who answers your question and waits for the next one.  
- **Agentic AI** = A project manager who takes your goal, breaks it into steps, uses the right tools, checks the work, and delivers a result.  

You say: *"Prepare a summary of last month's sales data and email it to the team."*

| Regular AI | Agentic AI |
|-----------|-----------|
| Writes a summary (if you give it data) | Pulls data from the database |
| Stops — you handle the rest | Writes the summary |
| | Formats it as an email |
| | Sends it to the team |
| | Confirms delivery |

---

### 3. The Four Capabilities of an Agent (3 minutes)

Every AI agent has (some or all of) these capabilities:

~~~ascii
+--------------------------------------------------+
|                   AI AGENT                        |
|                                                   |
|  1. REASONING    - Understands the goal           |
|                  - Breaks it into steps            |
|                                                   |
|  2. TOOL USE     - Calls APIs, databases,         |
|                    calculators, web search         |
|                                                   |
|  3. MEMORY       - Remembers past interactions     |
|                  - Stores facts for later          |
|                                                   |
|  4. PLANNING     - Decides next action             |
|                  - Adjusts if something fails      |
+--------------------------------------------------+
~~~

**Live Demo:**  
Ask ChatGPT:
~~~text
I need to plan a team lunch for 8 people next Friday. 
Budget is $200. We have 2 vegetarians. 
Find a restaurant, calculate per-person budget, and draft an invite email.
~~~
Watch how it breaks the task into steps — that's **reasoning + planning** in action (though without actual tool use).

---

### 4. Where Agentic AI Fits in the AI Landscape (2 minutes)

~~~ascii
Level 1: CHATBOT
  "Answer my question"
  Example: ChatGPT answering "What's the weather?"
         |
         v
Level 2: ASSISTANT
  "Help me with a task"
  Example: Copilot writing code based on comments
         |
         v
Level 3: AGENT
  "Achieve this goal"
  Example: AI that monitors logs, detects issues, 
           creates tickets, and alerts the team
         |
         v
Level 4: MULTI-AGENT SYSTEM
  "Multiple agents collaborate"
  Example: One agent researches, another writes, 
           a third reviews — like a virtual team
~~~

**Key takeaway:** We're moving from Level 1 → Level 3 today. Most business use cases are at Level 2–3.

---

### 5. Quick Check (1 minute)
**Question to the group:** "What's the difference between a chatbot and an agent in one sentence?"  
(Answer: A chatbot answers questions; an agent takes actions toward a goal.)

---

## 🧩 The Building Blocks: Tools, Memory & Planning (10 minutes)

### 1. Tools: Giving AI Hands (3 minutes)

By themselves, LLMs can only generate text. **Tools** let them interact with the real world.

**Common tools an agent can use:**

| Tool | What It Does | Example |
|------|-------------|---------|
| Calculator | Does math | "What's 15% tip on $83.50?" |
| Web Search | Finds current information | "What's Tesla's stock price today?" |
| Database Query | Reads/writes data | "Pull all orders from last week" |
| Email Sender | Sends messages | "Email the report to the team" |
| Code Executor | Runs code | "Run this Python script and show results" |
| File Reader | Reads documents | "Read the PDF and summarize it" |

**How tool use works internally:**

~~~ascii
User: "What's 25 × 17?"

Agent's internal process:
1. "I need to calculate 25 × 17"
2. "I have a calculator tool available"
3. → Calls: calculator(25, 17)
4. ← Gets: 425
5. "The answer is 425"

Agent responds: "25 × 17 = 425"
~~~

**Analogy:** Tools are like apps on your phone. The AI is the person deciding which app to open and what to do with the result.

---

### 2. Memory: Short-Term vs Long-Term (3 minutes)

We covered this briefly in the intro. Let's go deeper.

**Two types of agent memory:**

~~~ascii
SHORT-TERM MEMORY (Context Window)
+------------------------------------------+
| Everything in the current conversation    |
| - Your messages                           |
| - AI responses                            |
| - Tool results                            |
|                                           |
| LIMIT: Context window size (e.g., 128k)  |
| LIFESPAN: This session only               |
+------------------------------------------+

LONG-TERM MEMORY (External Storage)
+------------------------------------------+
| Stored in a database outside the LLM     |
| - User preferences                        |
| - Past conversation summaries             |
| - Important facts to remember             |
|                                           |
| LIMIT: Database size (virtually unlimited)|
| LIFESPAN: Across all sessions             |
+------------------------------------------+
~~~

**Why long-term memory matters for agents:**

Without memory:
~~~text
Session 1: "I prefer reports in bullet-point format."
Session 2: "Summarize this report."
→ AI generates a paragraph (forgot your preference)
~~~

With memory:
~~~text
Session 1: "I prefer reports in bullet-point format." → Stored in memory
Session 2: "Summarize this report."
→ AI checks memory → Generates bullet points
~~~

**Live Demo with ChatGPT:**
1. Open ChatGPT Settings → Memory → Check if enabled.  
2. Say: *"Remember that my team uses Monday.com for project management."*  
3. In a new chat, ask: *"What tool does my team use for project management?"*  
4. If memory is on → it remembers. If off → it doesn't.

---

### 3. Planning: The ReAct Pattern (3 minutes)

The most common pattern for AI agents is called **ReAct** — Reasoning + Acting.

~~~ascii
The ReAct Loop:

    +---> THINK (Reason about the task)
    |         |
    |         v
    |     ACT (Use a tool or take a step)
    |         |
    |         v
    |     OBSERVE (Check the result)
    |         |
    |         +---> Goal met? → DONE ✓
    |         |
    |         +---> Not done? → Loop back to THINK
    |                   |
    +-------------------+
~~~

**Concrete example:**

~~~text
Goal: "Find the cheapest flight from NYC to London next Friday"

THINK: I need to search for flights.
ACT:   → Use web search tool: "flights NYC to London next Friday"
OBSERVE: Found 5 options. Cheapest is $340 on Delta.

THINK: I should verify this price is current.
ACT:   → Use web search tool: "Delta NYC London next Friday price"
OBSERVE: Confirmed $340.

THINK: Goal met — I have the answer.
→ DONE: "The cheapest flight is $340 on Delta, departing Friday at 7 PM."
~~~

---

### 4. Quick Exercise (1 minute)
**Think of a task at work that has multiple steps.** Write down:
1. What tools would an agent need?  
2. What would it need to remember?  
3. What steps would it plan?  

---

## 🔧 Hands-On: Agentic Workflows with n8n (15 minutes)

### 1. What is n8n? (2 minutes)

**n8n** (pronounced "n-eight-n") is a free, open-source workflow automation tool. It lets you build **visual workflows** that connect AI to real tools — no coding required.

**Think of it as:** A visual way to build AI agents by connecting boxes together.

~~~ascii
n8n Workflow = Chain of Steps

[Trigger] → [Step 1] → [Step 2] → [Step 3] → [Output]
   |            |           |           |
  "When"      "Do"       "Then"      "Finally"
  something   this       do this     deliver
  happens                            result
~~~

---

### 2. Demo Workflow 1: AI Agent with Calculator (4 minutes)

**What we're building:** An AI that can do math by using a calculator tool.

~~~ascii
Workflow:

[Manual Trigger]
       |
       v
[Edit Fields]
  Set question: "What is 847 divided by 23, rounded to 2 decimals?"
       |
       v
[AI Agent]
  |-- Chat Model: OpenAI (GPT-4o mini)
  |-- Tool: Calculator
       |
       v
[Output]
  Answer: "847 ÷ 23 = 36.83"
~~~

**Step-by-step demo:**
1. Open n8n.  
2. Add a **Manual Trigger** node (this starts the workflow manually).  
3. Add an **Edit Fields** node → set a field called `question` with the math problem.  
4. Add an **AI Agent** node:
   - Connect a **Chat Model** (OpenAI, GPT-4o mini).  
   - Add the **Calculator** tool.  
5. Run the workflow.  
6. Watch the agent **decide** to use the calculator and return the answer.

**Key point:** The AI doesn't do the math itself — it recognizes it needs a tool and **calls** the calculator.

---

### 3. Demo Workflow 2: AI with Memory (5 minutes)

**What we're building:** A chatbot that remembers past conversations using a database.

~~~ascii
Workflow:

[Chat Trigger]
  "User sends a message"
       |
       v
[AI Agent]
  |-- Chat Model: OpenAI (GPT-4o)
  |-- Memory: PostgreSQL / SQLite Buffer
  |     |
  |     +-- Stores conversation history
  |     +-- Retrieves past messages
       |
       v
[Chat Response]
  "AI replies with context from past chats"
~~~

**Demo steps:**
1. Set up a **Chat Trigger** (activates when a message arrives).  
2. Add an **AI Agent** with a connected **Chat Model**.  
3. Add a **Memory** node (e.g., Window Buffer Memory or Postgres Chat Memory).  
4. Send: *"My name is Alex and I work in IT."*  
5. In the same conversation: *"What department do I work in?"*  
   → Agent recalls: "You work in IT."
6. **With persistent DB memory:** Close and reopen → ask again → still remembers.

**Without memory:** Each conversation starts fresh.  
**With memory:** Continuity across sessions.

---

### 4. Demo Workflow 3: Multi-Step Agent (3 minutes)

**What we're building:** An agent that researches a topic and drafts a summary.

~~~ascii
Workflow:

[Manual Trigger]
       |
       v
[Edit Fields]
  topic: "Benefits of AI in healthcare"
       |
       v
[AI Agent]
  |-- Chat Model: OpenAI (GPT-4o)
  |-- Tool 1: Web Search (SerpAPI)
  |-- Tool 2: Text Summarizer
       |
       v
[Output]
  "Here are the top 5 benefits of AI in healthcare, 
   based on recent articles: ..."
~~~

**What happens internally:**
~~~text
THINK: "I need to research AI in healthcare"
ACT:   → Web Search: "benefits of AI in healthcare 2024"
OBSERVE: Got 10 results with summaries

THINK: "I should summarize the key points"
ACT:   → Summarize the top findings into 5 bullet points
OBSERVE: Summary ready

→ DONE: Delivers formatted summary
~~~

---

### 5. Your Turn: Build a Simple Workflow (1 minute)

**Quick challenge** (to try after the session):
1. Create a workflow with a **Manual Trigger**.  
2. Add an **AI Agent** with a **Calculator** tool.  
3. Ask it: *"If I have 15 team members and a $3,000 budget for team lunch, how much per person?"*  
4. Verify the agent uses the calculator.

---

## 🏢 Real Workplace Use Cases & Patterns (10 minutes)

### 1. IT Operations (3 minutes)

**Use case:** Automated log monitoring and incident response.

~~~ascii
[Log Monitor]
  "New error detected in server logs"
       |
       v
[AI Agent]
  |-- Reads error details
  |-- Searches knowledge base for fix
  |-- Runs diagnostic command
       |
       v
[Decision]
  |-- Known issue? → Apply fix automatically
  |-- Unknown?    → Create ticket + alert on-call engineer
       |
       v
[Notification]
  "Slack: 'Server error detected. Auto-fix applied. Details: ...'"
~~~

**Real example:**  
- Error: "Disk space above 90%"  
- Agent: Checks which files are largest → deletes old temp files → confirms space freed → sends Slack message.

---

### 2. Business & HR (3 minutes)

**Use case:** Employee onboarding assistant.

~~~ascii
[New Hire Trigger]
  "HR enters new employee in system"
       |
       v
[AI Agent]
  |-- Creates accounts (email, Slack, tools)
  |-- Sends welcome email with first-day info
  |-- Schedules orientation meetings
  |-- Assigns training modules
       |
       v
[Checklist Tracker]
  "Onboarding progress: 4/4 tasks complete"
~~~

**Another example: Weekly Report Generator**
~~~text
Every Friday at 4 PM:
1. Agent pulls data from project management tool
2. Summarizes completed tasks and blockers
3. Formats as a weekly report
4. Emails to manager
~~~

---

### 3. Customer Support (2 minutes)

**Use case:** Intelligent ticket routing and auto-response.

~~~ascii
[Customer Email]
  "I can't log into my account"
       |
       v
[AI Agent]
  |-- Classifies: "Account access issue"
  |-- Checks FAQ/knowledge base
  |-- Finds: "Password reset instructions"
       |
       v
[Decision]
  |-- Simple fix? → Auto-reply with instructions
  |-- Complex?    → Route to human agent with context summary
~~~

---

### 4. Patterns to Remember (2 minutes)

| Pattern | Description | Example |
|---------|------------|---------|
| **Tool Augmentation** | AI uses external tools | Calculator, web search, APIs |
| **Retrieval-Augmented Generation (RAG)** | AI searches a knowledge base before answering | "Check our docs first, then answer" |
| **Workflow Chaining** | Multiple steps connected in sequence | Detect → Analyze → Fix → Report |
| **Human-in-the-Loop** | AI handles routine tasks, escalates edge cases | Auto-close simple tickets, flag complex ones |

---

## 🛡️ Risks, Pitfalls & Safeguards (5 minutes)

### 1. Infinite Loops (1 minute)

**Risk:** The agent keeps trying the same failing step forever.

~~~ascii
BAD:
THINK → ACT → FAIL → THINK → ACT → FAIL → THINK → ACT → FAIL → ...
(never stops)

GOOD:
THINK → ACT → FAIL → THINK → ACT → FAIL → STOP after 3 retries
→ Alert human: "I tried 3 times and failed. Here's what happened."
~~~

**Safeguard:** Always set **maximum retries** and **timeouts** on agentic workflows.

---

### 2. Wrong Tool Selection (1 minute)

**Risk:** The agent picks the wrong tool for the job.

**Example:**  
- Task: "Calculate 25 × 17"  
- Agent uses **web search** instead of **calculator** → gets a random web page  

**Safeguard:**  
- Write clear **tool descriptions** so the agent knows when to use each one.  
- Test: Does the agent pick the right tool for 10 different questions?  

---

### 3. Unsafe Actions (1 minute)

**Risk:** The agent takes a destructive action (e.g., deletes files, sends emails to wrong people).

**Safeguard:**  
- Use **approval steps** for high-risk actions (e.g., "Agent wants to send an email to 500 people — approve?").  
- Start with **read-only tools** (search, read) before granting **write tools** (send, delete).  

~~~ascii
Safety Ladder:

Level 1: READ ONLY     → Agent can search and read (safe)
Level 2: SUGGEST        → Agent suggests actions, human approves
Level 3: ACT + CONFIRM  → Agent acts, then confirms with human
Level 4: FULL AUTONOMY  → Agent acts independently (use with caution)
~~~

---

### 4. Stale Memory (1 minute)

**Risk:** The agent remembers outdated information and uses it incorrectly.

**Example:**  
- Memory: "Project deadline is March 15"  
- Reality: Deadline was moved to April 1  
- Agent sends a reminder about the wrong date  

**Safeguard:** Set **expiration rules** on memory entries and periodically review stored data.

---

### 5. Summary of Risks & Safeguards (1 minute)

| Risk | Safeguard |
|------|-----------|
| Infinite loops | Set max retries and timeouts |
| Wrong tool use | Write clear tool descriptions; test with varied inputs |
| Unsafe actions | Use approval steps; start read-only |
| Stale memory | Set expiration rules; review stored data |
| Hallucinated actions | Validate outputs before execution |

---

## 📖 Glossary of Key Terms

- **Agentic AI:** AI that can reason, plan, use tools, and act autonomously toward a goal.  
- **Agent:** A software system that uses an LLM plus tools/memory to complete multi-step tasks.  
- **Tool Use (Function Calling):** The ability of an AI to invoke external tools (APIs, calculators, databases).  
- **ReAct (Reasoning + Acting):** A pattern where the agent thinks, acts, observes, and loops until done.  
- **Short-Term Memory:** Information available in the current conversation (context window).  
- **Long-Term Memory:** Information stored externally (database) and recalled across sessions.  
- **n8n:** An open-source workflow automation tool for building visual AI agent workflows.  
- **RAG (Retrieval-Augmented Generation):** A pattern where AI searches a knowledge base before generating an answer.  
- **Human-in-the-Loop:** A design pattern where AI handles routine work but escalates to humans for edge cases.  
- **Workflow Chaining:** Connecting multiple automated steps in sequence to complete a task.  
- **Multi-Agent System:** Multiple AI agents collaborating, each with a specialized role.  
- **Guardrails:** Rules and limits set on an agent to prevent harmful or unintended actions.  

---

## 🔗 References

1. [n8n Documentation — AI Agents](https://docs.n8n.io/advanced-ai/)  
2. [LangChain — Introduction to Agents](https://python.langchain.com/docs/concepts/agents/)  
3. [OpenAI — Function Calling Guide](https://platform.openai.com/docs/guides/function-calling)  
4. [Anthropic — Tool Use with Claude](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)  
5. [Microsoft — AutoGen Multi-Agent Framework](https://microsoft.github.io/autogen/)  
6. [Google DeepMind — Gemini and Agentic Capabilities](https://deepmind.google/technologies/gemini/)  
7. [Lilian Weng — LLM-Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/)  

---

## ✅ Wrap-Up

- Agentic AI goes beyond chatbots: it can **reason, plan, use tools, and remember**.  
- The **ReAct pattern** (Think → Act → Observe → Repeat) is the core loop of most agents.  
- **n8n** lets you build agentic workflows visually — no code required.  
- Real workplace uses include IT monitoring, onboarding, reporting, and customer support.  
- Always apply safeguards: **retries, approvals, read-only start, and memory expiration**.  

**Next Steps for You:**  
1. Open n8n and build the calculator agent workflow from the demo.  
2. Think of one repetitive task at work — could an agent automate it?  
3. Map out the task: What tools would the agent need? What would it remember?  
4. Start small: build a **read-only** agent first, then add write capabilities after testing.  
