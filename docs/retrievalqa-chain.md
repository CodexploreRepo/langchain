# RetrievalQA Chain

## Introduction

- This part is to discuss on how to do question answering with the documents that we have just retrieved in **Retrieval**.
- `RetrievalQA` is to retrieve relevant text chunks with the input question to get the context and provide the context along with the question to the language model to get the answer.
- By default, `RetrievalQA` passes all the retrieved relevant chunks into the context window along with the original question and then to the same call to the language model.
  - _Problem_: In reality, the number of documents is high and they are not able to be fitted in the context window.
  - _Solution_: `MapReduce`, `Refine`, and `MapRerank`

<p align="center"><img src="../assets/img/langchain-retrievalqa.webp" width=400/><br>Retrieval QA Chain</p>

### Map Reduce, Refine, and MapRerank

- `map_reduce` involves four separate calls to the language model(ChatOpenAI in this case) for each of these retrieved relevant documents along with the original question
  - The result of these calls is combined in a final chain (`StuffedDocumentsChain`), which stuffs all these responses into the final call
  - `StuffedDocumentsChain` uses the system message, four summaries from the previous documents and the user question to get the answer.
- :star: `refine`: we get a better answer in the refine chain as it allows us to combine information sequentially leading to more carrying over of information than the MapReduce chain
  - first document + question &#8594; answer 1
  - answer 1 + second document + question &#8594; refine to get answer 2.
  - and so on until 4th document are used.

<p align="center"><img src="../assets/img/map_refine_maprerank.png" width=400/><br>RetrievalQA Chain with MapReduce, Reine and Map ReRank
</p>

#### Limitation

- Slower

## `RetrievalQA` Chain Implementation

- Step 1: Load the vector database that we persisted

```Python
# Load vector database that was persisted earlier and check collection count in it
from langchain.vectorstores import Chroma
from langchain.embeddings.openai import OpenAIEmbeddings
persist_directory = 'docs/chroma/'
embedding = OpenAIEmbeddings()
vectordb = Chroma(persist_directory=persist_directory, embedding_function=embedding)
print(vectordb._collection.count()) # get how many chunks in the vector db

# do a similarity search to check if the database is working properly.
question = "What are major topics for this class?"
docs = vectordb.similarity_search(question,k=3)
len(docs)
```

- Step 2: create the retriever from the `vectordb`
  - `search_type`: "similarity" or "mmr".
    - `search_type="similarity"` uses similarity search in the retriever object where it selects text chunk vectors that are most similar to the question vector.
    - `search_type="mmr"` uses the maximum marginal relevance search where it optimizes for similarity to query AND diversity among selected documents.

```Python
# retriever = vectordb.as_retriever(search_type="similarity", search_kwargs={"k":2})
retriever = vectordb.as_retriever(search_type="mmr", search_kwargs={"k":4, "fetch_k": 5})
```

- Step 3: initialise the language model

```Python
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)
```

### `RetrievalQA` Chain by default

- Initialise the `RetrievalQA` chain which does question answering backed by a retrieval step.
  - This is created by passing a language model and vector database as a retriever.

```Python
from langchain.chains import RetrievalQA

qa_chain = RetrievalQA.from_chain_type(
    llm,
    retriever=retriever
)
# Pass question to the qa_chain
question = "What are major topics for this class?"
result = qa_chain({"query": question})
result["result"]
```

### `RetrievalQA` Chain with Prompt

- Define the prompt tempalate about how to use the context.
  - It also has a placeholder for a `{context}` variable that retrieve from the vectorDB

```Python
from langchain.prompts import PromptTemplate

# Build prompt
template = """Use the following pieces of context to answer the question at the end. If you don't know the answer, just say that you don't know, don't try to make up an answer. Use three sentences maximum. Keep the answer as concise as possible. Always say "thanks for asking!" at the end of the answer.
{context}
Question: {question}
Helpful Answer:"""
QA_CHAIN_PROMPT = PromptTemplate.from_template(template)# Run chain

qa_chain_with_promt_template = RetrievalQA.from_chain_type(
    llm,
    retriever=retriever,
    return_source_documents=True, # True: to easily inspect the context docs
    chain_type_kwargs={"prompt": QA_CHAIN_PROMPT}
)

question = "Is probability a class topic?"
result = qa_chain_with_promt_template({"query": question})
# Check the result of the query
result["result"]
# Check the source document from where we
result["source_documents"][0]
```

### `RetrievalQA` Chain with Map Reduce, Refine, and MapRerank

- You can also define the chain type as one of the four options: "stuff", "map_reduce", "refine", "map_rerank".

```Python
qa_chain_mr = RetrievalQA.from_chain_type(
    llm,
    retriever=retriever,
    chain_type="map_reduce" # specify in chain_type "refine", "map_reduce"
)
result = qa_chain_mr({"query": question})
result["result"]
```

## Limitation of `RetrievalQA` Chain

- One of the biggest disadvantages of RetrievalQA chain is that the QA chain fails to preserve conversational history as RetrievalQA chain does not have any concept of state.
- In order for the chain to remember the previous question or previous answer, we need to introduce the concept of **memory**.
