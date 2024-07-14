# Daily Knowledge

## Day 1

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
