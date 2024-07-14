# Daily Knowledge

## Day 1

### Memory

- As Large Language Models are "stateless", LangChain provides several kinds of "memory" to store and accumulate the converstation.

#### Memory types

- `ConversationBufferMemory` allows for storing of messages and then extracts the messages in a variable
- `ConversationBufferWindowMemory` keeps a list of the interactions of the conversation over time.
  - It only uses the last `k` interactions
- `ConversationTokenBufferMemory` keeps a buffer of recent interactions in memory by token length rather than number of interactions to determine when to flush previous interaction tokens
- `ConversationSummaryMemory` creates a summary of the converstation over time
- **Vector data memory**: stores text (from converstaion or else where) in a vector databsae and retrieves the most relevant blocks of text
- **Entity memories**: using an LLM, it remembers details about specific
- Note 1: You can also use multiple memories at the same time.
  - For example: Converstation Memory + Entity Memory to recall individuals.
- Note 2: You also the converstation in a conventional DB such as key-value store or SQL.

### Prompt Templates

- LangChain’s templates use Python’s `str.format` by default, but for complex prompts you can also use [jinja2](https://palletsprojects.com/p/jinja/).
  - However, it is **NOT recommended** as jinja2 will cause the template very confusing. Unless we have a very good reason to use jinja2.

```Python
#Example using jinja2
jinja_prompt = PromptTemplate.from_template(
    "Tell me a {{adj}} jokes about {% if content %}{{content}}{% else %}yourself{% endif %}",
    template_format="jinja2"
)
jinja_prompt.format(adj="funny")
```
