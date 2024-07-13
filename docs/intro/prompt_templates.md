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
