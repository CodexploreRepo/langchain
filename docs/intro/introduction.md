# LangChain Introduction

## What is LangChain

- LangChain is an open source framework for building applications based on large language models (LLMs).
  - LLMs are large deep-learning models pre-trained on large amounts of data that can generate responses to user queries—for example, answering questions or creating images from text-based prompts.
- LangChain provides **tools** and **abstractions** to improve the customization, accuracy, and relevancy of the information the models generate.
  - For example, developers can use LangChain components to build new prompt chains or customize existing templates.
- LangChain also includes components that allow LLMs to _access new data sets without retraining_.

## LangChain's Components

- Lang Chain components include:
  - **Models**: there are 3 types of models
    - LLMs: 20+ integrations
    - Chat Models:
    - Text Embedding Models: 20+ integrations
  - **Prompts**: the style of creating inputs to pass into the models
    - Prompt Templates
    - Output Parsers: convert LLM output to more structured formats
      - Retry, Fixing Logic
    - Example Selectors: 5+ integrations
  - **Indexes**:
    - Document Loaders
    - Text Splitter
    - Vector Stores
    - Retrievers
  - **Chains**:
    - Prompt + LLM + Output Parsing
    - Can be building blocks for larger chains
  - **Agents**:
    - Agent Types: algorithm for getting LLMs to use tools
    - Agent Toolkit: agents armed with specific tools for a specific application
