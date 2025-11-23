Formatting Replies and Attaching Insights

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Data Connectivity and Email Services/Formatter_with_Agentic_Components.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/MN28rgMgUnuuTcwy5xBCPph" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---

### Communicating Results Effectively

In real-world applications, how information is presented is as important as the information itself. Agentic systems must format outputs into readable, actionable, and well-structured messages. This is especially critical when the agent sends these outputs via email.

This unit guides the reader through:

- Using GPT to summarize raw data

- Creating visual representations (e.g., charts) from query results

- Generating formatted content and sending it as attachments

---

### Structured Summaries with LangChain and GPT

Agents should use large language models to turn structured query results or text into email-ready summaries. LangChain’s LLMChain can be used to wrap this behavior:

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

summary_template = PromptTemplate(
    input_variables=["data"],
    template="""
You are an assistant generating summaries for email reports.
Summarize the following data clearly, concisely, and professionally:

{data}

Summary:
"""
)

llm = ChatOpenAI(model="gpt-4", temperature=0)
summarizer = LLMChain(llm=llm, prompt=summary_template)
```

Call this chain with structured or semi-structured input to produce a polished summary.