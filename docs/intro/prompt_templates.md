# LangChain's Prompt Templates

## What is Prompt Template ?

- Prompt template defines how prompts for LLMs should be structured, and provides opportunities for reuse and customization.
  - You can extend a template class for new use cases. These classes are called “templates” because they save you time and effort, and simplify the process of generating complex prompts.
- A LangChain's prompt template is a class containing elements you typically need for a Large Language Model (LLM) prompt. At a minimum, these includes:
  - A **natural language string that will serve as the prompt**: a simple text string, or, for prompts consisting of dynamic content, an f-string or docstring containing placeholders that represent variables.
  - **Formatting instructions** (optional): specify how dynamic content should appear in the prompt, i.e., whether it should be italicized, capitalized, etc.
  - **Input parameters** (optional): that you pass into the prompt class to provide instructions or context for generating prompts.
    - These variables for the placeholders in the string, whose values resolve to produce the final string that goes into the LLM through an API call as the prompt.

## Why use Prompt Template ?

- Prompts can be very long and detailed and re-use good prompts as you can.
- LangChain also provides prompts for common operations.

## Types of LangChain's Prompt Templates

- There are 3 types of LangChain's Prompt Templates
  - `PromptTemplate` for creating basic prompts.
  - `FewShotPromptTemplate` for few-shot learning.
  - `ChatPromptTemplate` for modeling chatbot interactions.
- Prompt types are designed for **flexibility**, _not exclusivity_, allowing you to blend their features, like _merging_ a `FewShotPromptTemplate` with a `ChatPromptTemplate`, to suit diverse use cases.
- LangChain’s templates use Python’s `str.format` by default, but for complex prompts you can also use [jinja2](https://palletsprojects.com/p/jinja/).

### `PromptTemplate`

- LangChain’s `PromptTemplate` class creates a dynamic string with variable placeholders

```Python
from langchain.prompts import PromptTemplate

prompt_template = PromptTemplate.from_template(
  "Write a delicious recipe for {dish} with a {flavor} twist."
)

# Formatting the prompt with new content
formatted_prompt = prompt_template.format(dish="pasta", flavor="spicy")
```

### `FewShotPromptTemplate`

- Few-shot learning is used to train models to do new tasks well, even when they have only a limited amount of training data available.
- Many real-world use cases benefit from few-shot learning:
  - Use Case 1 - _An automated fact checking tool_:
    - Provide different few-shot examples where the model is shown how to verify information, ask follow-up questions if necessary, and conclude whether a statement is true or false.
  - Use Case 2 - _A technical support and troubleshooting guide_:
    - Provide common troubleshooting steps, including how to ask the user for specific system details, interpret symptoms, and guide them through the solution process.
- How to create a `FewShotPromptTemplate`
  - Step 1: Create a list of few-shot examples.

```Python
from langchain_core.prompts.few_shot import FewShotPromptTemplate
from langchain_core.prompts.prompt import PromptTemplate

examples = [
    {
        "question": "Who lived longer, Muhammad Ali or Alan Turing?",
        "answer": """
Are follow up questions needed here: Yes.
Follow up: How old was Muhammad Ali when he died?
Intermediate answer: Muhammad Ali was 74 years old when he died.
Follow up: How old was Alan Turing when he died?
Intermediate answer: Alan Turing was 41 years old when he died.
So the final answer is: Muhammad Ali
""",
    },
    {
        "question": "When was the founder of craigslist born?",
        "answer": """
Are follow up questions needed here: Yes.
Follow up: Who was the founder of craigslist?
Intermediate answer: Craigslist was founded by Craig Newmark.
Follow up: When was Craig Newmark born?
Intermediate answer: Craig Newmark was born on December 6, 1952.
So the final answer is: December 6, 1952
""",
    },
    {
        "question": "Who was the maternal grandfather of George Washington?",
        "answer": """
Are follow up questions needed here: Yes.
Follow up: Who was the mother of George Washington?
Intermediate answer: The mother of George Washington was Mary Ball Washington.
Follow up: Who was the father of Mary Ball Washington?
Intermediate answer: The father of Mary Ball Washington was Joseph Ball.
So the final answer is: Joseph Ball
""",
    },
]
```

- Step 2: Create a formatter for the few-shot examples
  - Configure a formatter, which is a `PromptTemplate` object, that will format the few-shot examples into a string.

```Python
example_prompt = PromptTemplate(
    input_variables=["question", "answer"], template="Question: {question}\n{answer}"
)
# view the the first example after formatted with "example_prompt"
print(example_prompt.format(**examples[0]))

# Question: Who lived longer, Muhammad Ali or Alan Turing?

# Are follow up questions needed here: Yes.
# Follow up: How old was Muhammad Ali when he died?
# Intermediate answer: Muhammad Ali was 74 years old when he died.
# Follow up: How old was Alan Turing when he died?
# Intermediate answer: Alan Turing was 41 years old when he died.
# So the final answer is: Muhammad Ali

```

- Step 3: create a `FewShotPromptTemplate` object that takes
  - Few-shot examples
  - Formatter for the few-shot examples.

```Python
prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    suffix="Question: {input}",
    input_variables=["input"],
)

print(prompt.format(input="Who was the father of Mary Ball Washington?"))
```

### `ChatPromptTemplate`

- `ChatPromptTemplate` class focuses on the _conversation flow_ between a user and an AI system, and provides instructions or requests for roles:
  - `system` for a system chat message setting the stage (e.g., “You are a knowledgeable historian”).
  - `user` which contains the user’s specific historical _question_.
  - `AI` which contains the LLM’s preliminary _response_ or follow-up _question_.

```Python
from langchain_core.prompts import ChatPromptTemplate

# Define roles and placeholders
chat_template = ChatPromptTemplate.from_messages(
  [
    ("system", "You are a knowledgeable AI assistant. You are called {name}."),
    ("user", "Hi, what's the weather like today?"),
    ("ai", "It's sunny and warm outside."),
    ("user", "{user_input}"),
   ]
)

messages = chat_template.format_messages(name="Alice", user_input="Can you tell me a joke?")
```

## Prompt Template Examples

- Email Marketing
  - **Theme**: Effective Email Campaigns
  - **Explanation**: Utilize LLM to generate personalized email content for various customer segments.
  - **Importance**: Personalized emails are more likely to be opened and can drive higher conversion rates.
  - **Sample Prompt**: "Generate a personalized email for returning customers introducing our new sustainable products."
- Promotional Campaigns
  - **Theme**: Designing Effective Promotions
  - **Explanation**: Craft prompts to create innovative promotional campaigns.
  - **Importance**: Creative promotions can significantly increase visibility and consumer interest.
  - **Sample Prompt**: "Plan a month-long social media campaign for the launch of our new eco-friendly skincare range."
- Engaging Product Descriptions
  - **Theme**: Crafting Compelling Product Narratives
  - **Explanation**: Use ChatGPT to write appealing product descriptions.
  - **Importance**: Effective descriptions can significantly boost interest and sales by highlighting product benefits appealingly.
  - **Sample Prompt**: "Create an engaging and informative product description for a new eco-friendly facial cleanser."
- Content Creation for Social Media
  - **Theme**: Social Media Engagement
  - **Explanation**: Develop prompts that generate creative and engaging social media posts.
  - **Importance**: Social media presence is vital for reaching a broader audience and driving engagement.
  - **Sample Prompt**: "Create a series of tweets promoting the benefits of sustainable skincare routines."
- Analyzing Consumer Feedback
  - **Theme**: Feedback Analysis
  - **Explanation**: Use ChatGPT to extract and summarize customer feedback from various channels.
  - **Importance**: Understanding consumer feedback helps refine product offerings and marketing strategies.
  - **Sample Prompt**: "Summarize the main points of customer reviews received in the last month for our new eco-friendly skincare line."

## Reference

- [A Guide to Prompt Templates in LangChain](https://www.mirascope.io/post/langchain-prompt-template)
