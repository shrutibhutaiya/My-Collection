### Traditional Fine-Tuning / Retraining

Suppose you train an LLM on company knowledge available in January.

Then in February:

* New knowledge articles are published
* New product documentation is added
* Policies change

The model **does not automatically know** this new information.

To learn it, you'd need:

* Collect new training data
* Fine-tune again
* Validate
* Redeploy

This is expensive and slow.

---

### How RAG Handles New Knowledge

With RAG (Retrieval-Augmented Generation), the knowledge is stored outside the model.

Architecture:

```
User Question
      ↓
Retriever
      ↓
Vector DB / Knowledge Base
      ↓
Relevant Documents
      ↓
LLM
      ↓
Answer
```

When a new article arrives:

```
New Article
     ↓
Chunk
     ↓
Create Embeddings
     ↓
Store in Vector DB
```

That's it.

No model retraining required.

The next user query can immediately retrieve that article.

---

### Enterprise Example

Imagine you're building an internal ServiceNow support assistant.

Knowledge Base:

| Date   | Article                   |
| ------ | ------------------------- |
| Jan 1  | VPN setup guide           |
| Jan 5  | PTO policy                |
| Jan 10 | Deal Assist documentation |

Then today:

| Date   | Article                  |
| ------ | ------------------------ |
| Jun 16 | New AI Governance policy |

You don't retrain the LLM.

You simply:

1. Detect new KB article
2. Generate embedding
3. Insert into vector store
4. Update metadata

Now users can ask:

> "What is the new AI governance policy?"

The retriever finds the new document and provides it to the LLM.

---

### What Actually Gets Updated?

There are two separate things:

#### 1. LLM Weights

Examples:

* GPT
* Claude
* Llama

These are fixed.

Rarely updated.

#### 2. Vector Database

Examples:

* Pinecone
* Weaviate
* OpenSearch
* Databricks Vector Search
* Snowflake Cortex Search

This gets updated continuously.

For enterprise RAG systems, 99% of updates happen here.

---

### Typical Enterprise Pipeline

For knowledge articles:

```
Knowledge Article Created
            ↓
Event Trigger
            ↓
Chunk Document
            ↓
Generate Embeddings
            ↓
Update Vector Store
            ↓
Available to RAG
```

Many companies run this:

* Real-time
* Every hour
* Nightly batch

depending on freshness requirements.

---

### When Would You Need Retraining?

Retraining/fine-tuning is needed when you want to change the model's behavior, not its knowledge.

Examples:

❌ New KB article → No retraining

❌ New policy document → No retraining

❌ New product docs → No retraining

✅ Teach company-specific writing style → Fine-tuning may help

✅ Improve classification accuracy → Fine-tuning may help

✅ Teach a new workflow/reasoning pattern → Fine-tuning may help

---

### One Limitation of Basic RAG

If a user asks:

> "What changed in our AI governance policy between April and June?"

Basic RAG may retrieve both documents but not reason perfectly over differences.

In enterprise systems, people often add:

* Hybrid search (keyword + vector)
* Metadata filtering
* Knowledge graphs
* Agentic retrieval
* Re-ranking models

on top of RAG.

### For your Enterprise AI use cases

For things like:

* Success stories
* Knowledge articles
* Product documentation
* Sales playbooks
* Meeting notes
* Deal guidance

A RAG system would typically **not retrain the model at all**. You would continuously ingest new documents into the vector store, and the LLM remains unchanged. This is one reason RAG became the default architecture for enterprise GenAI applications: knowledge changes daily, while model retraining is expensive and operationally heavy.