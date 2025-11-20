Formatting Replies and Attaching Insights

https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Formatter_with_Agentic_Components.ipynb

Learning Objectives

By the end of this unit, the reader will be able to:

Generate structured GPT responses for email delivery

Format summaries and visualizations for readability

Generate charts from query data using matplotlib

Use LangChain LLMChains to summarize and prepare insights

Communicating Results Effectively

In real-world applications, how information is presented is as important as the information itself. Agentic systems must format outputs into readable, actionable, and well-structured messages. This is especially critical when the agent sends these outputs via email.

This unit guides the reader through:

Using GPT to summarize raw data

Creating visual representations (e.g., charts) from query results

Generating formatted content and sending it as attachments

Structured Summaries with LangChain and GPT

Agents should use large language models to turn structured query results or text into email-ready summaries. LangChain’s LLMChain can be used to wrap this behavior:


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


Call this chain with structured or semi-structured input to produce a polished summary.