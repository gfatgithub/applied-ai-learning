# Applied AI Learning — Deep Dive: Retrieval-Augmented Generation (RAG)

## 📅 Duration: 1 Hour  
**Agenda with Timings**
- 0:00 – 0:05 → Welcome & Learning Objectives  
- 0:05 – 0:15 → The Problem RAG Solves  
- 0:15 – 0:30 → How RAG Works (Step by Step)  
- 0:30 – 0:40 → Building a RAG Pipeline (Live Demo)  
- 0:40 – 0:50 → Advanced RAG Patterns & Pitfalls  
- 0:50 – 0:55 → Glossary of Key Terms  
- 0:55 – 1:00 → References & Wrap-Up  

---

## 🎯 Learning Objectives
By the end of this session, you will be able to:
1. Explain what RAG is and why it exists.  
2. Describe the full RAG pipeline: ingestion, chunking, embedding, retrieval, generation.  
3. Identify when RAG is the right approach vs. fine-tuning or prompt engineering.  
4. Build a simple RAG pipeline using real tools.  
5. Recognize common RAG failure modes and apply fixes.  

---

## 🧩 The Problem RAG Solves (10 minutes)

### 1. Why LLMs Alone Aren't Enough (3 minutes)

LLMs are trained on public data up to a cutoff date. They don't know about:
- **Your company's internal documents** (policies, wikis, procedures).  
- **Recent events** (anything after their training cutoff).  
- **Private data** (customer records, internal reports).  

When you ask an LLM about something it doesn't know, it either says "I don't know" or — worse — **halluccinates a confident-sounding wrong answer**.

**The core problem:**  
> "How do I give an LLM access to MY data without retraining the entire model?"

**Answer: Retrieval-Augmented Generation (RAG).**

---

### 2. What Is RAG? (2 minutes)

RAG is a pattern where you **retrieve relevant documents first**, then **feed them to the LLM** as context so it can generate an answer grounded in your actual data.

**Analogy:**  
- **Without RAG** = Asking someone a question and hoping they memorized the answer.  
- **With RAG** = Giving someone a reference book and saying "find the answer in here, then explain it to me."  

~~~ascii
Without RAG:                          With RAG:
+--------+     +---------+           +--------+     +----------+     +---------+
| User   | --> | LLM     | --> ?     | User   | --> | Search   | --> | LLM +   |
| Query  |     | (memory |           | Query  |     | Your     |     | Your    |
|        |     |  only)  |           |        |     | Documents|     | Context |
+--------+     +---------+           +--------+     +----------+     +---------+
                                                          |               |
                                                    "Here are the    "Based on the
                                                     relevant         retrieved docs,
                                                     passages"        here's your answer"
~~~

---

### 3. RAG vs. Fine-Tuning vs. Prompt Engineering (3 minutes)

| Approach | When to Use | Cost | Data Freshness |
|----------|------------|------|----------------|
| **Prompt Engineering** | General tasks, small context | Low | Real-time (you paste it in) |
| **RAG** | Large knowledge bases, docs that change often | Medium | Near real-time (re-index) |
| **Fine-Tuning** | Teaching the model a new style, domain, or format | High | Static (retrain to update) |

**Decision rule of thumb:**  
1. Start with **prompt engineering** (can you just paste the info in the prompt?).  
2. If the context is too large or changes often → **RAG**.  
3. If RAG isn't capturing the right *style/behavior* → **fine-tuning**.  

**Analogy:**  
- **Prompt engineering** = Giving someone a sticky note with instructions.  
- **RAG** = Giving someone access to a filing cabinet.  
- **Fine-tuning** = Sending someone to a training course.  

---

### 4. Wrap-Up Check (2 minutes)
**Quick Poll:** "Your team has a 500-page employee handbook that updates quarterly. Users want to ask questions about it. Which approach do you use?"  
(Answer: RAG — too large for prompt, changes too often for fine-tuning.)

---

## ⚙️ How RAG Works — Step by Step (15 minutes)

### 1. The RAG Pipeline Overview (2 minutes)

RAG has two phases: **Ingestion** (prepare your data) and **Query** (answer questions).

~~~ascii
=== PHASE 1: INGESTION (Done Once / Periodically) ===

+------------+     +-----------+     +------------+     +-----------------+
| Your       | --> | Chunking  | --> | Embedding  | --> | Vector          |
| Documents  |     | (split    |     | (convert   |     | Database        |
| (PDF, Wiki,|     |  into     |     |  chunks to |     | (store vectors  |
|  HTML, etc)|     |  pieces)  |     |  numbers)  |     |  for search)    |
+------------+     +-----------+     +------------+     +-----------------+

=== PHASE 2: QUERY (Every Time a User Asks) ===

+--------+     +------------+     +-----------+     +---------+     +--------+
| User   | --> | Embed the  | --> | Search    | --> | Build   | --> | LLM    |
| Query  |     | Query      |     | Vector DB |     | Prompt  |     | Answer |
|        |     | (same      |     | (find top |     | (query +|     |        |
|        |     |  model)    |     |  matches) |     |  docs)  |     |        |
+--------+     +------------+     +-----------+     +---------+     +--------+
~~~

---

### 2. Step 1: Document Loading (2 minutes)

First, you gather all the documents you want the LLM to know about.

**Common sources:**
- PDF files (policies, reports)  
- Confluence / SharePoint wikis  
- Markdown documentation  
- Database records  
- Emails or ticketing systems  

**Key point:** Garbage in, garbage out. If your source documents are poorly written, outdated, or contradictory, RAG will reflect that.

---

### 3. Step 2: Chunking (3 minutes)

Documents are split into smaller pieces called **chunks**. This is one of the most important steps.

**Why chunk?**  
- LLMs have a limited context window (e.g., 128K tokens for GPT-4o).  
- You want to retrieve only the **relevant** parts, not entire documents.  
- Smaller chunks = more precise retrieval.  

~~~ascii
Original Document (50 pages):
+------------------------------------------------------------------+
| Chapter 1: Onboarding Policy                                     |
| Chapter 2: Benefits Overview                                     |
| Chapter 3: PTO Policy                                            |
| Chapter 4: Remote Work Guidelines                                 |
| ...                                                               |
+------------------------------------------------------------------+
                    |
                    v  (Chunking)
+------------------+  +------------------+  +------------------+
| Chunk 1:         |  | Chunk 2:         |  | Chunk 3:         |
| "New employees   |  | "Health benefits |  | "PTO accrual     |
|  must complete   |  |  include medical |  |  begins at 15    |
|  Form I-9 within |  |  dental, and     |  |  days per year   |
|  3 business days"|  |  vision plans.." |  |  for new hires.."|
+------------------+  +------------------+  +------------------+
~~~

**Chunking strategies:**

| Strategy | How It Works | Best For |
|----------|-------------|----------|
| **Fixed size** | Split every N characters/tokens | Simple, fast |
| **Sentence/paragraph** | Split on natural boundaries | Readable chunks |
| **Recursive** | Try paragraph → sentence → character | Most common default |
| **Semantic** | Use AI to find topic boundaries | Complex documents |

**Common mistake:** Chunks too large (retrieves irrelevant text) or too small (loses context).  
**Rule of thumb:** Start with 500–1000 tokens per chunk with 100-token overlap.

---

### 4. Step 3: Embedding (3 minutes)

Each chunk is converted into a **vector** — a list of numbers that captures its meaning.

~~~ascii
Chunk: "PTO accrual begins at 15 days per year for new hires"
                    |
                    v  (Embedding Model)
Vector: [0.23, -0.87, 0.45, 0.12, -0.33, 0.91, ... ] (1536 numbers)
~~~

**Why vectors?**  
- Vectors let us do **semantic search** — finding content by meaning, not keywords.  
- "vacation days" and "PTO accrual" are far apart as keywords but close as vectors.  

**Popular embedding models:**

| Model | Provider | Dimensions | Notes |
|-------|----------|-----------|-------|
| text-embedding-3-small | OpenAI | 1536 | Good balance of cost and quality |
| text-embedding-3-large | OpenAI | 3072 | Higher quality, higher cost |
| Cohere embed-v3 | Cohere | 1024 | Strong multilingual support |
| all-MiniLM-L6-v2 | Open Source | 384 | Free, runs locally |
| nomic-embed-text | Open Source | 768 | Free, strong performance |

---

### 5. Step 4: Vector Storage (2 minutes)

Vectors are stored in a **vector database** — a specialized database optimized for similarity search.

**Popular vector databases:**

| Database | Type | Best For |
|----------|------|----------|
| **Pinecone** | Cloud managed | Production, zero ops |
| **Weaviate** | Cloud / self-host | Hybrid search |
| **ChromaDB** | Local / embedded | Prototyping, small projects |
| **pgvector** | PostgreSQL extension | Teams already using Postgres |
| **Qdrant** | Cloud / self-host | High performance |
| **FAISS** | In-memory library | Research, prototyping |

**For getting started:** ChromaDB or pgvector are great choices — minimal setup.

---

### 6. Step 5: Retrieval & Generation (3 minutes)

When a user asks a question:

~~~ascii
User asks: "How many PTO days do new hires get?"
                    |
                    v
Step A: Embed the query using the SAME embedding model
        Query vector: [0.21, -0.85, 0.47, ...]
                    |
                    v
Step B: Search the vector database for similar chunks
        Match 1 (score 0.94): "PTO accrual begins at 15 days per year..."
        Match 2 (score 0.87): "After 5 years, PTO increases to 20 days..."
        Match 3 (score 0.72): "PTO requests must be submitted 2 weeks..."
                    |
                    v
Step C: Build the prompt
        +------------------------------------------------------+
        | System: You are a helpful HR assistant. Answer using  |
        | ONLY the provided context. If the answer is not in    |
        | the context, say "I don't have that information."     |
        |                                                        |
        | Context:                                                |
        | [Chunk 1 text]                                          |
        | [Chunk 2 text]                                          |
        | [Chunk 3 text]                                          |
        |                                                        |
        | User: How many PTO days do new hires get?               |
        +------------------------------------------------------+
                    |
                    v
Step D: LLM generates answer grounded in the retrieved context
        "New hires receive 15 PTO days per year. After 5 years
         of employment, this increases to 20 days per year."
~~~

**Key design choices:**
- **Top-K:** How many chunks to retrieve (typically 3–5).  
- **Score threshold:** Minimum similarity to include (e.g., 0.7).  
- **Prompt instruction:** Always tell the LLM to answer from context only.  

---

## 🛠️ Building a RAG Pipeline — Live Demo (10 minutes)

### Demo 1: Simple RAG with Python (5 minutes)

Here's a minimal RAG pipeline using LangChain and ChromaDB:

~~~python
# Step 1: Install dependencies
# pip install langchain langchain-openai langchain-community chromadb

from langchain_community.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_community.vectorstores import Chroma
from langchain.chains import RetrievalQA

# Step 2: Load your documents
loader = TextLoader("company_handbook.txt")
documents = loader.load()

# Step 3: Chunk the documents
splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,      # tokens per chunk
    chunk_overlap=100    # overlap between chunks
)
chunks = splitter.split_documents(documents)
print(f"Split into {len(chunks)} chunks")

# Step 4: Create embeddings and store in ChromaDB
embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
vectorstore = Chroma.from_documents(chunks, embeddings)

# Step 5: Create a retrieval chain
llm = ChatOpenAI(model="gpt-4o", temperature=0)
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
    return_source_documents=True
)

# Step 6: Ask a question
result = qa_chain.invoke({"query": "How many PTO days do new hires get?"})
print(result["result"])
print("Sources:", [doc.metadata for doc in result["source_documents"]])
~~~

**Expected output:**
~~~text
New hires receive 15 PTO days per year, which begins accruing from their 
start date. After 5 years of employment, PTO increases to 20 days per year.

Sources: [{'source': 'company_handbook.txt'}, {'source': 'company_handbook.txt'}, ...]
~~~

---

### Demo 2: RAG with n8n (No-Code) (5 minutes)

For teams that prefer a visual workflow:

~~~ascii
n8n RAG Workflow:

+----------------+     +------------------+     +------------------+
| Trigger:       | --> | Vector Store     | --> | AI Agent         |
| Chat Message   |     | Retriever        |     | (GPT-4o)         |
| from user      |     | (Pinecone /      |     |                  |
|                |     |  Supabase)       |     | System prompt:   |
+----------------+     +------------------+     | "Answer from     |
                              |                  |  context only"   |
                              v                  +------------------+
                        +-----------+                   |
                        | Returns   |                   v
                        | top 3     |            +-----------+
                        | matching  |            | Response  |
                        | chunks    |            | to user   |
                        +-----------+            +-----------+

Setup Steps in n8n:
1. Add "Chat Trigger" node
2. Add "Vector Store Retriever" node → connect to Pinecone
3. Add "AI Agent" node → model: GPT-4o
4. Connect: Trigger → Retriever → Agent
5. In Agent system prompt: "Answer based only on the provided context"
6. Test with a question about your documents
~~~

---

## 🔬 Advanced RAG Patterns & Pitfalls (10 minutes)

### 1. Common RAG Failures (4 minutes)

| Failure | Symptom | Root Cause | Fix |
|---------|---------|------------|-----|
| **Wrong chunks retrieved** | Answer is about wrong topic | Poor chunking or embedding | Improve chunk boundaries; add metadata filters |
| **Answer not in context** | LLM says "I don't know" when it shouldn't | Chunks too small; missed relevant content | Increase chunk overlap; retrieve more chunks (top-K) |
| **Hallucination despite context** | LLM invents info not in chunks | Weak system prompt | Add "ONLY use provided context" instruction |
| **Outdated answers** | Info is stale | Documents not re-indexed | Automate re-ingestion on a schedule |
| **Slow responses** | Takes 5+ seconds | Large chunks, many retrieved | Reduce chunk size; use faster embedding model |

---

### 2. Advanced Patterns (4 minutes)

**Pattern 1: Hybrid Search (Keyword + Semantic)**
~~~ascii
User Query: "Form I-9 deadline"
                |
          +-----+-----+
          |           |
    Semantic       Keyword
    Search         Search
    (meaning)      (exact match)
          |           |
          +-----+-----+
                |
         Combine & Re-rank
                |
         Top results
~~~
- Semantic search finds meaning ("employment verification form").  
- Keyword search catches exact terms ("I-9").  
- **Best for:** Technical docs, legal documents, anything with specific terminology.  

**Pattern 2: Query Transformation**
~~~ascii
User asks: "What's our return policy?"
                    |
                    v
LLM rewrites as multiple queries:
  1. "Product return policy and timeline"
  2. "Refund conditions and requirements"  
  3. "Exchange and store credit policy"
                    |
                    v
Each query retrieves chunks → merge → deduplicate → answer
~~~
- Catches related information the user didn't explicitly ask for.  

**Pattern 3: Re-Ranking**
~~~ascii
Initial retrieval: 20 chunks (fast, approximate)
                    |
                    v
Re-ranker model scores each for relevance to the query
                    |
                    v
Top 3 most relevant chunks → sent to LLM
~~~
- Tools: Cohere Rerank, cross-encoder models.  
- **Why:** Initial vector search is fast but approximate. Re-ranking improves precision.  

---

### 3. RAG Evaluation: How Do You Know It's Working? (2 minutes)

| Metric | What It Measures | How to Check |
|--------|-----------------|--------------|
| **Retrieval Precision** | Are the retrieved chunks relevant? | Manually review top-K results |
| **Answer Faithfulness** | Does the answer match the source? | Compare answer to retrieved chunks |
| **Answer Relevance** | Does the answer address the question? | Human review or LLM-as-judge |
| **Context Recall** | Did retrieval find ALL relevant chunks? | Compare against known-good answers |

**Tools for RAG evaluation:**
- **Ragas** — open-source framework for automated RAG evaluation.  
- **LangSmith** — tracing + evaluation for LangChain pipelines.  
- **TruLens** — feedback functions for RAG quality.  

---

## 📖 Glossary of Key Terms

| Term | Definition |
|------|-----------|
| **RAG** | Retrieval-Augmented Generation — a pattern that retrieves relevant documents before generating an answer |
| **Chunk** | A piece of a document, split for processing and retrieval |
| **Embedding** | A numerical representation (vector) of text that captures its meaning |
| **Vector Database** | A database optimized for storing and searching vectors by similarity |
| **Semantic Search** | Finding content by meaning rather than keyword matching |
| **Top-K** | The number of most-similar chunks to retrieve |
| **Hybrid Search** | Combining keyword search and semantic search |
| **Re-ranking** | Using a second model to re-score retrieved results for relevance |
| **Ingestion** | The process of loading, chunking, embedding, and storing documents |
| **Context Window** | The maximum amount of text an LLM can process in one request |
| **Hallucination** | When an LLM generates information not supported by the provided context |
| **Faithfulness** | Whether the generated answer accurately reflects the source documents |

---

## 📚 References

1. [LangChain RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/) — Official step-by-step guide  
2. [Pinecone RAG Guide](https://www.pinecone.io/learn/retrieval-augmented-generation/) — Concepts and implementation  
3. [ChromaDB Documentation](https://docs.trychroma.com/) — Getting started with local vector storage  
4. [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings) — How to create and use embeddings  
5. [Ragas Documentation](https://docs.ragas.io/) — RAG evaluation framework  
6. [n8n AI Agent Documentation](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/) — No-code RAG workflows  
7. [Chunking Strategies by Pinecone](https://www.pinecone.io/learn/chunking-strategies/) — Comprehensive chunking guide  

---

## 🏁 Wrap-Up & Next Steps (5 minutes)

### Key Takeaways
1. **RAG = Retrieve then Generate** — give the LLM your data as context, don't hope it memorized it.  
2. **Chunking is critical** — bad chunks = bad answers. Start with 500–1000 tokens, recursive splitting.  
3. **Always instruct the LLM** to answer from context only — prevents hallucinations.  
4. **Start simple** — ChromaDB + LangChain or n8n can get you a working prototype in hours.  
5. **Measure quality** — use retrieval precision and answer faithfulness to know if it's working.  

### Action Items
- [ ] Identify one document set at your workplace that would benefit from RAG (e.g., internal wiki, policy docs).  
- [ ] Try building a simple RAG prototype with ChromaDB and a sample document.  
- [ ] Review the chunking strategies and pick one appropriate for your documents.  
- [ ] Set up a simple evaluation: ask 10 known questions and check if the answers are correct.  

### What's Next?
In the next module (**06 — Evaluating AI Outputs**), we'll learn how to systematically measure whether AI outputs are good — essential for taking RAG (or any AI system) from prototype to production.
