# Applied AI Learning — Deep Dive: Test-Time Compute

## 📅 Duration: 1 Hour  
**Agenda with Timings**
- 0:00 – 0:05 → Welcome & Learning Objectives  
- 0:05 – 0:15 → What is Test-Time Compute? (Concepts & Why It Matters)  
- 0:15 – 0:25 → How Models "Think Harder": Techniques Behind Test-Time Compute  
- 0:25 – 0:40 → Hands-On: Seeing Test-Time Compute in Action (Live Demos)  
- 0:40 – 0:50 → Cost, Speed & Quality Trade-Offs  
- 0:50 – 0:55 → Pitfalls, Safeguards & Best Practices  
- 0:55 – 0:58 → Glossary of Key Terms  
- 0:58 – 1:00 → References & Wrap-Up  

---

## 🎯 Learning Objectives
By the end of this session, you will be able to:
1. Explain what test-time compute is and how it differs from training-time compute.  
2. Describe techniques models use to "think harder" at inference time.  
3. Recognize real examples of test-time compute in tools you already use.  
4. Make informed decisions about when to use more vs. less compute.  
5. Balance cost, speed, and quality when choosing AI models and settings.  

---

## ⚡ What is Test-Time Compute? (10 minutes)

### 1. Training Time vs Test Time (3 minutes)

AI models have two major phases of compute:

~~~ascii
PHASE 1: TRAINING TIME (Before you ever use the model)
+-----------------------------------------------------+
| Happens once (then the model is "ready")             |
|                                                      |
| - Feed billions of text documents                    |
| - Model adjusts millions/billions of parameters      |
| - Takes weeks/months on thousands of GPUs            |
| - Costs: $10M – $100M+ for large models              |
|                                                      |
| Analogy: A student spending 4 years in university     |
+-----------------------------------------------------+

PHASE 2: TEST TIME / INFERENCE TIME (When you use the model)
+-----------------------------------------------------+
| Happens every time you send a prompt                  |
|                                                      |
| - Model processes your input                         |
| - Generates output token by token                    |
| - Takes seconds to minutes                           |
| - Costs: fractions of a cent to dollars per query     |
|                                                      |
| Analogy: The graduate answering questions at work     |
+-----------------------------------------------------+
~~~

**Key insight:** "Test-time compute" is about how much computing power the model uses **when answering your question** — not during training.

**Why it matters:** More test-time compute can produce **better, more accurate answers** — but it costs more and takes longer.

---

### 2. The Big Idea: Thinking Fast vs Thinking Slow (3 minutes)

This concept is inspired by psychologist Daniel Kahneman's framework:

~~~ascii
SYSTEM 1: Fast Thinking (Low Compute)
+------------------------------------------+
| Quick, automatic, instinctive             |
| "What's 2 + 2?" → "4" (instant)          |
|                                           |
| AI equivalent:                            |
| - Standard model response                |
| - One pass, one answer                   |
| - Fast and cheap                         |
+------------------------------------------+

SYSTEM 2: Slow Thinking (High Compute)
+------------------------------------------+
| Deliberate, careful, step-by-step         |
| "What's 17 × 23 + 15% tax?" → needs work |
|                                           |
| AI equivalent:                            |
| - Model "thinks" through multiple steps   |
| - Checks its own work                    |
| - Takes more time, costs more            |
| - Better for complex problems            |
+------------------------------------------+
~~~

**Analogy:**  
- **Low test-time compute** = Answering a trivia question from memory. Fast but might be wrong.  
- **High test-time compute** = Working through a math problem on a whiteboard. Slower but more reliable.  

---

### 3. Real-World Example (2 minutes)

**Scenario:** You ask an AI to review a 10-page contract for risks.

| Approach | Compute Level | What Happens | Quality |
|----------|--------------|--------------|---------|
| Quick scan | Low | Model reads once, gives high-level summary | May miss details |
| Standard analysis | Medium | Model reads carefully, identifies key clauses | Good for most cases |
| Deep review | High | Model reads multiple times, cross-references clauses, checks for contradictions, generates detailed report | Thorough, catches subtle issues |

**The takeaway:** You don't need "deep review" compute for every question. Match the compute level to the task importance.

---

### 4. Quick Check (2 minutes)
**Question to the group:** "When was the last time you needed a 'quick answer' vs a 'carefully researched answer'? How did you decide?"

This is the same decision AI systems make — and test-time compute is how they do it.

---

## 🧠 How Models "Think Harder": Techniques (10 minutes)

### 1. Chain-of-Thought Reasoning (3 minutes)

The simplest form of test-time compute: making the model **show its work**.

~~~ascii
WITHOUT Chain-of-Thought (Fast, Low Compute):

User: "A store buys 100 items at $5 each and sells them at $8 each. 
       30 items remain unsold. What's the profit?"
AI:   "$90" 
      (Might be wrong — no visible reasoning)

WITH Chain-of-Thought (Slower, Higher Compute):

User: "A store buys 100 items at $5 each and sells them at $8 each. 
       30 items remain unsold. What's the profit?
       Think step by step."
AI:   "Step 1: Cost = 100 × $5 = $500
       Step 2: Items sold = 100 - 30 = 70
       Step 3: Revenue = 70 × $8 = $560
       Step 4: Profit = $560 - $500 = $60
       
       The profit is $60." ✓
~~~

**Why it uses more compute:** The model generates more tokens (the reasoning steps), which means more computation per query.

---

### 2. Self-Consistency: Multiple Attempts (2 minutes)

**The idea:** Ask the model to solve the same problem **multiple times**, then pick the most common answer.

~~~ascii
Attempt 1: "The profit is $60" ←
Attempt 2: "The profit is $60" ← Same answer 3 times
Attempt 3: "The profit is $60" ←    = high confidence
Attempt 4: "The profit is $90"
Attempt 5: "The profit is $60" ←

Final answer: $60 (appeared 4 out of 5 times)
~~~

**Analogy:** Like asking 5 different people to solve the same math problem independently. If 4 out of 5 get the same answer, you trust it more.

**Compute cost:** 5× a single query (you run the model 5 times).

---

### 3. Tree-of-Thought: Exploring Multiple Paths (2 minutes)

**The idea:** Instead of reasoning in a single line, the model **explores multiple reasoning paths** like a decision tree.

~~~ascii
Problem: "Find the best route from A to D"

Single Chain (Standard):
A → B → C → D  (one path — might not be best)

Tree of Thought (Multiple Paths):
         A
        /|\
       / | \
      B  C  E
      |  |  |
      C  D  F
      |     |
      D     D
      
Evaluate all paths:
  A→B→C→D = 15 miles
  A→C→D   = 10 miles  ← Best! ✓
  A→E→F→D = 20 miles
~~~

**Compute cost:** Much higher — exploring multiple branches simultaneously.  
**When it's worth it:** Complex planning, strategy, or optimization problems.

---

### 4. Reasoning Models: Built-In Deep Thinking (2 minutes)

Some newer models are **specifically designed** to use more test-time compute automatically:

| Model | How It Works | Best For |
|-------|-------------|----------|
| OpenAI o1 | Internally "thinks" before responding — spends more tokens reasoning | Math, science, coding, logic |
| OpenAI o1-mini | Lighter version of o1 — faster, cheaper, still reasons | Quick logic and coding tasks |
| OpenAI o3 | Most advanced reasoning model — deep multi-step thinking | Complex research, analysis |
| Claude 3.5 Sonnet + "Extended Thinking" | Can be prompted to think through problems more carefully | Research, long documents |

**Key point:** These models **automatically** spend more compute on harder problems. You don't have to ask them to "think step by step" — they do it internally.

~~~ascii
Standard Model:
Input → [Quick Processing] → Output
(fast, cheap, works for most tasks)

Reasoning Model (o1, o3):
Input → [Internal Reasoning: 10-50x more tokens] → Output
(slower, more expensive, much better for hard problems)
~~~

---

### 5. Quick Summary (1 minute)

| Technique | Compute Level | How It Works |
|-----------|--------------|--------------|
| Direct answer | Low | Model answers immediately |
| Chain-of-thought | Medium | Model shows reasoning steps |
| Self-consistency | High | Model runs multiple attempts, picks best |
| Tree-of-thought | Very High | Model explores multiple reasoning paths |
| Reasoning models | Automatic | Model internally decides how hard to think |

---

## 🛠️ Hands-On: Seeing Test-Time Compute in Action (15 minutes)

### 1. Demo: Simple vs Complex Questions (4 minutes)

**Try these two prompts in ChatGPT and compare response times:**

**Prompt 1 (Low compute):**
~~~text
"What is the capital of France?"
~~~
→ Instant response: "Paris."  
→ Uses very little compute.

**Prompt 2 (High compute):**
~~~text
"A farmer has a 500-acre farm. He plants corn on 40% of it, wheat on 
25%, and soybeans on the rest. Corn yields 150 bushels/acre at $4/bushel, 
wheat yields 50 bushels/acre at $6/bushel, and soybeans yield 45 bushels/acre 
at $10/bushel. Calculate the total revenue from each crop and the overall 
total. Show your work step by step."
~~~
→ Longer response time, step-by-step reasoning.  
→ Uses much more compute.

**What to observe:** The second prompt takes noticeably longer and produces more tokens (the reasoning steps).

---

### 2. Demo: Comparing Standard vs Reasoning Models (4 minutes)

**If you have access to ChatGPT with o1/o3 or GPT-4o:**

Ask this tricky logic problem to **both** GPT-4o and o1:

~~~text
"There are three boxes: one has only apples, one has only oranges, 
and one has both. Each box is labeled, but ALL labels are wrong. 
You can pick one fruit from one box (without looking inside). 
What's the minimum strategy to correctly label all boxes?"
~~~

**GPT-4o (standard model):**  
Might give a correct answer, but sometimes struggles with the logic.

**o1 (reasoning model):**  
Internally reasons through the possibilities, almost always gets it right.

~~~text
Expected answer:
"Pick from the box labeled 'Both'. 
- If you get an apple, that box is 'Apples Only' (since the label is wrong).
- The box labeled 'Apples' must be 'Oranges Only' (all labels are wrong).
- The box labeled 'Oranges' must be 'Both'.
One pick is enough to label all three boxes correctly."
~~~

---

### 3. Demo: Scaling Compute with Self-Consistency (3 minutes)

**Demonstrate asking the same question multiple times:**

~~~text
"Estimate how many piano tuners are in Chicago. Think step by step 
and show your calculations."
~~~

Ask this **3 times** (in separate chats or by regenerating the response).

**What to observe:**
- Each response takes a different approach but arrives at a similar estimate.  
- If 2 out of 3 responses agree on ~200–300 tuners, that's a high-confidence range.  
- This is self-consistency in action.

---

### 4. Demo: When NOT to Use Extra Compute (2 minutes)

**The point:** Not every task benefits from more compute.

| Task | Extra Compute Needed? | Why |
|------|----------------------|-----|
| "What's the weather in New York?" | No | Simple factual lookup |
| "Write a haiku about Monday" | No | Creative but simple |
| "Translate 'hello' to Spanish" | No | Direct translation |
| "Analyze this 50-page legal contract for risks" | Yes | Complex, multi-step reasoning |
| "Debug this 200-line Python script" | Yes | Requires tracing logic flow |
| "Am I making a logical error in this argument?" | Yes | Requires careful analysis |

**Rule of thumb:** If you could answer it in 5 seconds yourself, the AI doesn't need extra compute either.

---

### 5. Group Exercise (2 minutes)

**For each task below, decide: Low, Medium, or High compute?**

1. "Summarize this 1-paragraph email" → ___  
2. "Find a logical flaw in this 3-page business proposal" → ___  
3. "Translate this sentence to French" → ___  
4. "Calculate the optimal inventory order for 50 products across 3 warehouses" → ___  
5. "Write a birthday message for a coworker" → ___  

**Answers:** 1-Low, 2-High, 3-Low, 4-High, 5-Low

---

## 💰 Cost, Speed & Quality Trade-Offs (10 minutes)

### 1. The Three-Way Trade-Off (3 minutes)

Every AI query involves a balance of three factors:

~~~ascii
          QUALITY
           /\
          /  \
         /    \
        /  You \
       /  pick  \
      /   two    \
     /____________\
   COST          SPEED

Fast + Cheap    = Lower quality (quick drafts)
Fast + Quality  = Expensive (premium models, high compute)
Cheap + Quality = Slow (batch processing, smaller models with more passes)
~~~

**Analogy:** It's like the old saying: "Fast, cheap, or good — pick two."

---

### 2. Real Cost Comparison (3 minutes)

| Model/Approach | Speed | Cost per 1K tokens | Quality for Hard Tasks |
|----------------|-------|-------------------|----------------------|
| GPT-4o mini | ⚡ Very fast | ~$0.00015 (input) | Good for simple tasks |
| GPT-4o | ⚡ Fast | ~$0.0025 (input) | Great for most tasks |
| Claude 3.5 Sonnet | ⚡ Fast | ~$0.003 (input) | Excellent for long docs |
| OpenAI o1-mini | 🐢 Slower | ~$0.003 (input) | Excellent for logic/math |
| OpenAI o1 | 🐢🐢 Slow | ~$0.015 (input) | Outstanding for complex reasoning |

**Practical example:**

| Scenario | Best Choice | Est. Cost | Time |
|----------|------------|-----------|------|
| Answer 100 simple FAQs | GPT-4o mini | ~$0.05 | Seconds each |
| Summarize a 20-page report | GPT-4o | ~$0.15 | 10-15 seconds |
| Debug a complex code issue | o1-mini | ~$0.30 | 30-60 seconds |
| Analyze a legal contract | o1 | ~$2-5 | 1-3 minutes |

---

### 3. Decision Framework (2 minutes)

~~~ascii
START: What's your task?
         |
    +----+----+
    |         |
Simple task?  Complex reasoning?
    |              |
    v              v
 GPT-4o mini    Is speed critical?
 or GPT-4o         |
                +--+--+
                |     |
              Yes?   No?
                |     |
                v     v
           GPT-4o    o1 or o1-mini
           with      (let it think)
           CoT prompt
~~~

**Rule of thumb:**
- **90% of tasks** → Standard model (GPT-4o, Claude) is plenty.  
- **9% of tasks** → Add chain-of-thought or few-shot prompting for a boost.  
- **1% of tasks** → Use a reasoning model (o1, o3) for truly hard problems.

---

### 4. Live Demo: Compute Budget Planning (2 minutes)

**Scenario:** Your team uses AI for 3 types of tasks daily:

| Task | Daily Volume | Model Choice | Cost/Query | Daily Cost |
|------|-------------|--------------|------------|------------|
| Quick email replies | 50 queries | GPT-4o mini | $0.001 | $0.05 |
| Report summaries | 10 queries | GPT-4o | $0.05 | $0.50 |
| Complex analysis | 2 queries | o1-mini | $0.50 | $1.00 |
| **Total** | **62 queries** | | | **$1.55/day** |

Monthly cost: ~$1.55 × 22 workdays = **~$34/month**

**Key insight:** Using the right model for the right task saves money while maintaining quality.

---

## 🛡️ Pitfalls, Safeguards & Best Practices (5 minutes)

### 1. Pitfall: Using a Sledgehammer for a Nail (1 minute)

**Problem:** Using the most expensive, most powerful model for every task.

**Example:**  
Using o1 ($0.015/1K tokens) to answer "What's 2 + 2?" when GPT-4o mini ($0.00015/1K tokens) works just as well.  
That's **100× the cost** for the same answer.

**Safeguard:** Start with the cheapest model. Upgrade only if the quality isn't good enough.

~~~ascii
Tier 1: Try GPT-4o mini first (cheapest)
         |
    Quality OK? --Yes--> DONE ✓
         |
        No
         |
Tier 2: Try GPT-4o (mid-range)
         |
    Quality OK? --Yes--> DONE ✓
         |
        No
         |
Tier 3: Try o1/o1-mini (premium reasoning)
         |
         DONE ✓
~~~

---

### 2. Pitfall: Ignoring Latency (1 minute)

**Problem:** Using a reasoning model in a real-time chat where users expect instant replies.

**Example:**  
User asks a simple question in a customer support chatbot. The o1 model takes 30 seconds to reason through it. User gets frustrated and leaves.

**Safeguard:**  
- Use **fast models** for real-time interactions (chat, customer support).  
- Use **reasoning models** for background/batch tasks (analysis, reports).

---

### 3. Pitfall: Not Measuring Quality Improvements (1 minute)

**Problem:** Spending more on compute without checking if it actually improves results.

**Safeguard:** Run a simple A/B test:
1. Ask 10 questions to the standard model.  
2. Ask the same 10 questions to the reasoning model.  
3. Rate the answers (1–5 scale).  
4. Compare: Is the quality improvement worth the cost difference?

---

### 4. Best Practices Summary (2 minutes)

| Practice | Description |
|----------|------------|
| **Start cheap** | Use the smallest model that works for your task |
| **Add reasoning when needed** | Use chain-of-thought prompting before upgrading models |
| **Match compute to importance** | Trivial task = low compute; critical task = high compute |
| **Monitor costs** | Track token usage and costs weekly |
| **Test before committing** | Always compare model outputs before choosing |
| **Use batch for heavy tasks** | Process large analyses overnight, not in real-time |

---

## 📖 Glossary of Key Terms

- **Test-Time Compute (Inference Compute):** The computing resources used when running an AI model to generate a response.  
- **Training-Time Compute:** The computing resources used to build/train the model (happens once, before deployment).  
- **Inference:** The process of an AI model generating output based on input — the "using" phase.  
- **Latency:** The time between sending a prompt and receiving the response.  
- **Chain-of-Thought (CoT):** A prompting technique where the model shows its reasoning step by step.  
- **Self-Consistency:** Running the same prompt multiple times and selecting the most frequent answer.  
- **Tree-of-Thought (ToT):** A technique where the model explores multiple reasoning paths like a decision tree.  
- **Reasoning Model:** A model specifically designed to spend more compute on harder problems (e.g., o1, o3).  
- **Token:** A small chunk of text (~3–4 characters) — the unit of cost and computation.  
- **Throughput:** How many queries a model can handle per second.  
- **Batch Processing:** Running many queries together (often cheaper) rather than one at a time.  
- **Scaling Law:** The principle that more compute (training or inference) generally improves AI performance.  

---

## 🔗 References

1. [OpenAI — o1 Model Overview](https://openai.com/index/introducing-openai-o1-preview/)  
2. [Hugging Face — What is Test-Time Compute and How to Scale It?](https://huggingface.co/blog/Kseniase/testtimecompute)  
3. [Anthropic — Research on Scaling](https://www.anthropic.com/research)  
4. [Google DeepMind — Scaling Laws for AI](https://deepmind.google/discover/blog/)  
5. [Chain-of-Thought Prompting Paper (Wei et al., 2022)](https://arxiv.org/abs/2201.11903)  
6. [Tree of Thoughts Paper (Yao et al., 2023)](https://arxiv.org/abs/2305.10601)  
7. [OpenAI — API Pricing](https://openai.com/api/pricing/)  

---

## ✅ Wrap-Up

- **Test-time compute** is the compute used when the AI generates a response — not during training.  
- More compute can mean better answers, but also higher costs and slower responses.  
- Techniques range from simple (**chain-of-thought**) to advanced (**tree-of-thought**, **reasoning models**).  
- **Match compute to task complexity** — don't use a sledgehammer for a nail.  
- Start with cheap models and upgrade only when quality demands it.  

**Next Steps for You:**  
1. Try the "simple vs complex" demo: compare response times for easy vs hard questions.  
2. Add "Think step by step" to your next complex prompt and compare the output quality.  
3. Check your team's AI usage: are you using the right model tier for each task type?  
4. Estimate your monthly AI cost using the budget framework from this session.  
