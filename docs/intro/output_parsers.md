# Output Parsers

## Introduction

- Output Parser will have method `get_format_instructions()` from the base class `BaseOutputParser` and it is a **Runnable**, so we can just put that parser into the chain.

## `CommaSeparatedListOutputParser`

- `CommaSeparatedListOutputParser` is to parse the LLM output string into a Python List.
- Define the LLM model

```Python
from langchain.chat_models import ChatOpenAI
# To control the randomness and creativity of the generated
# text by an LLM, use temperature = 0.0
chat_model = ChatOpenAI(temperature=0.0, model="gpt-3.5-turbo")
```

- Define the output parser and get the `format_instructions`

```Python
from langchain.output_parsers import CommaSeparatedListOutputParser

output_parser = CommaSeparatedListOutputParser()
format_instructions = output_parser.get_format_instructions()
prompt = ChatPromptTemplate.from_template(
    template="List five {subject}.\n{format_instructions}"
)
# fill the message with input text & format_instructions
messages = prompt.format_messages(subject="ice cream flavors", format_instructions=format_instructions)
```

- Provide formatted message to the LLM & parse the LLM's output

```Python
response = chat(messages)
output_list = output_parser.parse(response.content)
print(output_list)
# ['Vanilla', 'Chocolate', 'Strawberry', 'Mint Chocolate Chip', 'Cookies and Cream']
```

## `DatetimeOutputParser`

```Python
output_parser = DatetimeOutputParser(Hformat="%a %d/%m/%Y")
prompt = PromptTemplate(
    template="{question}\n{format_instructions}",
    input_variables=["question"],
    partial_variables={"format_instructions": format_instructions}
)
print(prompt)
# {question}
# Write a datetime string that matches the following pattern: '%a %d/%m/%Y'.

# Examples: Tue 30/08/591, Mon 07/05/249, Fri 04/05/1708
```

- Add the output_parser into the chain along with the prompt included the `format_instructions`

```Python
model = VertexAI()
chain = prompt | model | output_parser
ans = chain.invoke({"question":"When is Lunar New Year of 2024?"})
print(ans)
# 2024-02-10 00:00:00
```

- The `output_parser` reformat it to the default format of “%Y-%m-%dT%H:%M:%S.%fZ” instead of the format that we specify in the `DatetimeOutputParser` which is `"%a %d/%m/%Y"`

## Reference

- [LangChain 101 — Lesson 3: Output Parser](https://medium.com/@larry_nguyen/langchain-101-lesson-3-output-parser-406591b094d7)
