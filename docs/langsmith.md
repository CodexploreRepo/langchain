# Lang Smith

If you wish to experiment on the `LangSmith platform` (previously known as LangChain Plus):

- Go to [LangSmith](https://www.langchain.com/langsmith) and sign up
- Create an API key from your account's settings
- Use this API key in the code below

```Python
import os
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_ENDPOINT"] = "https://api.langchain.plus"
os.environ["LANGCHAIN_API_KEY"] = "..." # replace dots with your api key
```
