# Conversational Retrieval Chain

## Introduction

- `ConversationalRetrievalChain` is very similar to `RetrievalQA`.
  - It added an additional parameter `chat_history` to pass in chat history which can be used for follow-up questions.
- In summary: ConversationalRetrievalChain = Conversation memory + RetrievalQA Chain

<p align="center"><img src="../assets/img/conversational-retrieval-chain.png" width=250/></p>

## `ConversationalRetrievalChain` Implementation

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

- Step 4: create the prompt template
  - The prompt to use to condense the chat history and new question into a standalone question.

```Python
from langchain.prompts import PromptTemplate

# Build prompt
template = """Use the following pieces of Context to answer the question at the end. You can improve your answer from previous answers in History. If you don't know the answer, just say that you don't know, don't try to make up an answer. Use three sentences maximum. Keep the answer as concise as possible. Always say "thanks for asking!" at the end of the answer.
Context: {context}
History: {chat_history}
Question: {question}
Helpful Answer:"""
QA_CHAIN_PROMPT = PromptTemplate.from_template(template)# Run chain
```

### Memory

- `ConversationBufferMemory` is a memory that allows for storing messages and then extracts the messages in a variable.
  - `memory_key=chat_history` your chain (and likely your prompt) should expect an input named `chat_history`

```Python
from langchain.memory import ConversationBufferMemory
# keep a buffer of chat message history
memory = ConversationBufferMemory(
    memory_key="chat_history",
    return_messages=True # return the chat history as list of msgs
)

# addtional demo:
memory1 = ConversationBufferMemory( memory_key="history")
memory1.save_context({"input": "hi"}, {"output": "whats up"})
print(f"memory1 ==> {memory1.load_memory_variables({})}")

memory2 = ConversationBufferMemory( memory_key="chat_history")
memory2.save_context({"input": "hi"}, {"output": "whats up"})
print(f"memory2 ==> {memory2.load_memory_variables({})}")
# memory1 ==> {'history': 'Human: hi\nAI: whats up'}
# memory2 ==> {'chat_history': 'Human: hi\nAI: whats up'}
```

### `ConversationalRetrievalChain`

```Python
from langchain.chains import ConversationalRetrievalChain
retriever=vectordb.as_retriever()
qa = ConversationalRetrievalChain.from_llm(
    llm,                   # LLM
    retriever=retriever,   # Retriever
    memory=memory          # Memory
    chain_type="refine"    # Chain Type: stuff
    condense_question_prompt=QA_CHAIN_PROMPT, # prompt template
    return_source_documents=True,
    return_generated_question=True,
)
```
