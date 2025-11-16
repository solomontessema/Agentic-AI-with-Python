## 2.1: LangChain Object Model

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Building with LangChain/Building_a_Minimal_Chain_App.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

### Understanding LangChain's Modular Architecture

LangChain is a framework that connects large language models (LLMs) with real-world applications by offering modular components for reasoning, memory, tool usage, and control flow. Each module encapsulates a specific responsibility and can be composed to build intelligent systems.

LangChain's object model is highly decoupled and abstracts core AI functions:

- **Language understanding and generation** via LLMs  
- **Instruction formatting** via Prompt Templates  
- **Reasoning pipelines** via Chains  
- **External action interfaces** via Tools  
- **Decision-making entities** via Agents  

This modular design supports both simple task automation and complex agentic behaviors.

---

## Core Components in LangChain

### **LLMs (Language Models)**

LLM objects wrap communication with model APIs and handle configuration such as model name, temperature, and system instructions.

```python
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model="gpt-4", temperature=0)
```

---

### **PromptTemplate**

PromptTemplates structure the input passed to the LLM, allowing variables to be substituted at runtime. They ensure consistent prompt formatting across tasks.

```python
from langchain.prompts import PromptTemplate
prompt = PromptTemplate(
    input_variables=["question"],
    template="Answer the following: {question}"
)
```
---

### **Chains**

Chains represent sequences of processing steps. Outputs from one step flow into the next, allowing structured reasoning pipelines.

```python
from langchain.chains import LLMChain
chain = LLMChain(llm=llm, prompt=prompt)
```
---

### **Tools**

Tools provide interfaces to external systems—APIs, databases, search engines, calculators, etc. Agents call these tools when necessary.

---

### **Agents**

Agents use reasoning loops to determine how to act. They decide which tools to use, when to use them, and how to integrate results into responses.

---

## LangChain’s Object Hierarchy

1. **PromptTemplate** → formats instructions  
2. **LLM** → generates text from prompts  
3. **LLMChain** → binds prompts and models together  
4. **SequentialChain / SimpleSequentialChain** → builds multi-step flows  
5. **Tool** → exposes capabilities to agents  
6. **Agent** → orchestrates decisions, tools, and prompts  

Introspection tools like `help()` or attribute inspection reveal structure, methods, and configurations for each object.

---

## Chain-First vs. Agent-First Design Patterns

### **Chain-First**
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

| Aspect        | Chain-First | Agent-First |
|---------------|-------------|-------------|
| Flow Control  | Fixed       | Dynamic     |
| Flexibility   | Low         | High        |
| Complexity    | Lower       | Higher      |
| Suitable For  | Linear Tasks | Adaptive Workflows |
