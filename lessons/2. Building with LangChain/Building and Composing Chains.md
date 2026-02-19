# 2.2: Building and Composing Chains

## The Role of Chains in LangChain

In LangChain, chains are built using **LangChain Expression Language (LCEL)** — a declarative, composable syntax using the `|` pipe operator. LCEL provides a unified `Runnable` interface that is transparent, streamable, and production-ready.

LCEL chains are ideal for scenarios such as:

- Summarizing data
- Formatting outputs
- Transforming inputs
- Chaining multiple LLM calls

Every component in LCEL — prompts, models, parsers, and custom functions — implements the `Runnable` interface, sharing a consistent API: `.invoke()`, `.stream()`, and `.batch()`. The `|` operator composes these runnables left-to-right, passing the output of each step as input to the next.

---

## Creating a Basic Chain for Question Answering

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser

llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

prompt = PromptTemplate(
    input_variables=["topic"],
    template="What are the latest trends in {topic}?"
)

chain = prompt | llm | StrOutputParser()
response = chain.invoke({"topic": "machine learning"})
print(response)
```

This is the foundational pattern: a prompt feeds into the model, and a parser extracts the text response.

---

## Composing Multi-Step Logic

You can pipe chains together to build multi-stage pipelines. When the output of one chain needs to be reshaped before entering the next, use a `RunnableLambda` to remap it.

**Example: Summarize → Rephrase**

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableLambda

llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

summarize_prompt = PromptTemplate(
    input_variables=["input_text"],
    template="Summarize this content: {input_text}"
)

rephrase_prompt = PromptTemplate(
    input_variables=["text"],
    template="Rephrase this summary professionally: {text}"
)

summarize_chain = summarize_prompt | llm | StrOutputParser()
rephrase_chain = rephrase_prompt | llm | StrOutputParser()

pipeline = (
    summarize_chain
    | RunnableLambda(lambda summary: {"text": summary})
    | rephrase_chain
)

output = pipeline.invoke({"input_text": "The global AI market is projected to grow significantly over the next decade."})
print(output)
```

The `RunnableLambda` in the middle remaps the string output of the first chain into the dict shape the next prompt expects.

---

## Advanced Composition with Named Intermediate Outputs

When you need to preserve and selectively pass intermediate values across multiple steps, use `RunnablePassthrough.assign()`. This accumulates outputs into a shared dict as it flows through the chain, keeping all intermediate results accessible.

**Use Case: Extract Entities → Summarize → Format Response**

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

entity_prompt = PromptTemplate(
    input_variables=["input_text"],
    template="Extract all people, organizations, and locations from the following: {input_text}"
)

summary_prompt = PromptTemplate(
    input_variables=["entities"],
    template="Summarize the following extracted entities: {entities}"
)

format_prompt = PromptTemplate(
    input_variables=["summary"],
    template="Format this into a user-friendly paragraph: {summary}"
)

entity_chain = entity_prompt | llm | StrOutputParser()
summary_chain = summary_prompt | llm | StrOutputParser()
format_chain = format_prompt | llm | StrOutputParser()

sequential_chain = (
    RunnablePassthrough.assign(entities=entity_chain)
    | RunnablePassthrough.assign(summary=summary_chain)
    | RunnablePassthrough.assign(final_output=format_chain)
)

result = sequential_chain.invoke({"input_text": "OpenAI and Google DeepMind are competing in developing AGI."})
print(result["final_output"])
```

`RunnablePassthrough.assign()` takes the current dict, runs the provided chain against it, and merges the result back under the given key. Each step enriches the shared dict, making all intermediate outputs available downstream for debugging, logging, or reuse.

This modular architecture integrates cleanly with LangSmith tracing and supports streaming out of the box.