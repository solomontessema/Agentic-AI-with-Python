## 2.1: LangChain Object Model

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Building with LangChain/Building_a_Minimal_Runnable.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/JpY9BQ4CSZuRn7JNdYg35" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---

### Understanding LangChain's Modular Architecture

LangChain is a framework that connects large language models (LLMs) with real-world applications by offering modular components for reasoning, memory, tool usage, and control flow. Each module encapsulates a specific responsibility and can be composed to build intelligent systems.

LangChain's object model is highly decoupled and abstracts core AI functions:

- **Language understanding and generation** via LLMs  
- **Instruction formatting** via Prompt Templates  
- **Reasoning pipelines** via Runnables (LangChain Expression Language)  
- **External action interfaces** via Tools  
- **Decision-making entities** via Agents  

This modular design supports both simple task automation and complex agentic behaviors.

---

## Core Components in LangChain

### **LLMs (Language Models)**

LLM objects wrap communication with model APIs and handle configuration such as model name, temperature, and system instructions.

```python
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model="gpt-4", temperature=0)
```

---

### **PromptTemplate**

PromptTemplates structure the input passed to the LLM, allowing variables to be substituted at runtime. They ensure consistent prompt formatting across tasks.

```python
from langchain_core.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_template(
    "Answer the following: {question}"
)
```
---

### **Runnables and LCEL (LangChain Expression Language)**

Runnables are the modern building blocks of LangChain pipelines, replacing the older `Chain` classes. They implement a common interface (`invoke`, `stream`, `batch`) and are composed using the **pipe operator (`|`)**, forming what is called a **LangChain Expression Language (LCEL)** chain.

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

llm = ChatOpenAI(model="gpt-4", temperature=0)
prompt = ChatPromptTemplate.from_template("Answer the following: {question}")
output_parser = StrOutputParser()

# Compose a pipeline using the pipe operator
chain = prompt | llm | output_parser

# Invoke the pipeline
result = chain.invoke({"question": "What is the capital of France?"})
```

Every component in an LCEL chain is a `Runnable`. This means you can mix and match prompts, models, output parsers, and custom functions seamlessly. The `|` operator passes the output of one step as the input to the next.

> **Note:** The legacy `LLMChain` class is deprecated. Use LCEL with the pipe operator instead.

---

### **Tools**

Tools provide interfaces to external systems—APIs, databases, search engines, calculators, etc. Agents call these tools when necessary.

---

### **Agents**

Agents use reasoning loops to determine how to act. They decide which tools to use, when to use them, and how to integrate results into responses.

---

## LangChain's Object Hierarchy

1. **PromptTemplate** → formats instructions  
2. **LLM** → generates text from prompts  
3. **Runnable / LCEL Chain** → composes prompts, models, and parsers using `|`  
4. **Tool** → exposes capabilities to agents  
5. **Agent** → orchestrates decisions, tools, and prompts  

---

## Runnable-First vs. Agent-First Design Patterns

### **Runnable-First (formerly Chain-First)**
- Deterministic  
- Fixed workflow  
- Lower complexity  
- Ideal for summarization, translation, extraction tasks

### **Agent-First**
- Dynamic and adaptive  
- Higher flexibility  
- More complex  
- Ideal for autonomous research, customer support, planning tasks

---

### Comparison Table

| Aspect| Runnable-First| Agent-First|
| :--- | :--- | :--- |
| Flow Control| Fixed| Dynamic|
| Flexibility| Low| High|
| Complexity| Lower| Higher|
| Suitable For| Linear Tasks| Adaptive Workflows|
