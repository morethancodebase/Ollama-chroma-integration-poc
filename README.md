# 🧠 RAG with Ollama 3 + Chroma — Cricket Facts Demo

This project demonstrates how to integrate **Ollama 3 (local LLM)** with **Chroma (vector database)** to perform **Retrieval-Augmented Generation (RAG)** — where an LLM retrieves stored facts from a vector database before generating a response.

The example compares:
- 🧩 **Pure reasoning (LLM alone)**  
vs  
- 🧮 **Retrieval-augmented reasoning (LLM + Chroma DB)**  

using facts about the **ICC Men’s Cricket World Cup 2023** 🏏

---

## ⚙️ Tools & Technologies Used

| Component | Description | Example Used |
|------------|--------------|---------------|
| 🧩 **Ollama 3** | Local LLM runner (runs on `http://localhost:11434`) | Provides both chat and embedding APIs |
| 🗣️ **Chat Model** | LLM used for answering queries | `llama3` |
| 🔢 **Embedding Model** | Converts text into numerical vector embeddings | `mxbai-embed-large` |
| 🧮 **Vector Database** | Stores and searches embeddings for semantic similarity | `Chroma` |
| 🐍 **Language** | Implementation and orchestration | Python 3.9+ |

---

## 🧭 Flow Overview

Below is the conceptual diagram of how Ollama and Chroma interact in this RAG pipeline:

![RAG Flow](A_flowchart_in_the_image_illustrates_Retrieval-Aug.png)

### 🧩 Step-by-Step Explanation

1. **User Query:** User asks “Who won the ICC Men’s Cricket World Cup in 2023?”
2. **Embedding Generation:** The embedding model (`mxbai-embed-large`) converts stored facts into vectors.
3. **Vector Storage:** These vectors are stored in Chroma under the collection `cricket_facts`.
4. **Similarity Search:** The query is embedded and compared with stored vectors to find the most semantically similar facts.
5. **Context Injection:** The retrieved facts are passed to the chat model (`llama3`) as context.
6. **LLM Response:** Llama 3 answers *only based on retrieved data* (not its own internal knowledge).

---

## 🧩 Project Flow (Code Explanation)

| Step | Description | Example Output |
|------|--------------|----------------|
| **1. Initialize Chroma** | Create a collection called `cricket_facts`. | Empty DB initialized. |
| **2. Print DB before feeding data** | Shows that no embeddings or documents exist yet. | `IDs: [] Documents: None` |
| **3. Ask before feeding embeddings** | Query Llama 3’s own reasoning (no RAG). | “Australia won the ICC Cricket World Cup 2023.” |
| **4. Add facts into Chroma** | Feed multiple facts (venue, winner, runner-up). | Stored successfully with embeddings. |
| **5. Print DB after feeding** | Shows stored documents and sample embeddings. | `[‘Australia won the ICC Men’s Cricket World Cup in 2023.’]` |
| **6. Ask after feeding embeddings** | Create query embedding and search top 3 similar docs. | Retrieves relevant World Cup facts. |
| **7. Ask Llama with retrieved context** | Model uses only retrieved info to answer. | “Australia won the ICC Men’s Cricket World Cup 2023.” |

---

## 🧰 Installation

### 1️⃣ Install Ollama and Pull Models
```bash
brew install ollama
ollama pull llama3
ollama pull mxbai-embed-large
