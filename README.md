# LangChain

## Introduction

- [LangChain Introduction](./docs/intro/introduction.md)
- [Prompt Templates](./docs/intro/prompt_templates.md)

## RAG

- Indexing
  - [Document Loading](./docs/rag/document-loading.md)
  - [Document Splitting](./docs/rag/document-splitting.md)
  - [Embeddings & Vector Stores](./docs/rag/embeddings-vectorstores.md)
  <p align="center"><img src="./assets/img/rag_indexing.png" width=600/><br>RAG Indexing: Load, Split, Embed, Store</p>
- [Retrieval](./docs/retrieval.md)
  - [RetrivalQA Chain](./docs/rag/retrievalqa-chain.md): if the retrieved documents cannot fit into the context window, we can use `map_reduce`, `refine`, and `map_rerank` to solve this problem
    - Limitation of RetrivalQA: fails to preserve conversational history as RetrievalQA chain does not have any concept of state
  - [Conversational Retrieval Chain](./docs/rag/conversational-retrieval-chain.md) = Conversation memory + RetrievalQA Chain

<p align="center"><img src="./assets/img/rag_retrieval_generation.png" width=600/><br>Given a user input, relevant splits are retrieved from storage using a Retriever & ChatModel / LLM produces an answer using a prompt that includes the question and the retrieved data</p>

## Tools

- [LangSmith](./docs/langsmith.md)

## Installation

- Core LangChain: `pip install --upgrade langchain`
