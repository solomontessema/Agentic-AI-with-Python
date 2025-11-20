## 2.3: Prompt Engineering in LangChain

This lesson includes a hands-on coding exercise. After reviewing the material below, please open the lab assignment using the link below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Building with LangChain/Prompt_Flow_Refactor.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/U884dp3RDo5tBEDKFxcvo" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---

### Advanced Prompt Templates in LangChain

Prompt templates in LangChain allow developers to dynamically shape the inputs sent to LLMs. Beyond static instructions, prompts can be designed to accommodate contextual data, conditional paths, and adaptive phrasing.

- Prompt engineering becomes particularly powerful when prompts:

- Respond to intermediate results from previous steps

- Vary based on task type or data characteristics

- Structure outputs for tool or user consumption

LangChain's PromptTemplate class is highly flexible and supports formatting with any number of runtime variables.

```python
from langchain.prompts import PromptTemplate

prompt = PromptTemplate(
    input_variables=["question", "context"],
    template="""
    Use the following context to answer the question:
    Context: {context}

    Question: {question}
    Answer in one paragraph.
    """
)
```

The above template uses two dynamic inputs and guides output formatting.

---

### Passing Variables Through Chained Workflows

In chain-based architecture, prompt templates often receive variables from previous steps. This requires careful design to ensure variable names align across prompts.

For example, if the first chain extracts entities:
```python
entity_prompt = PromptTemplate(
    input_variables=["input_text"],
    template="Extract key entities from: {input_text}"
)
```

Then the second chain might consume its output:

```python
summary_prompt = PromptTemplate(
    input_variables=["entities"],
    template="Summarize these entities: {entities}"
)
```

When constructing a SequentialChain, the output key from the first prompt must match the input variable for the next.

```python
entity_chain = LLMChain(llm=llm, prompt=entity_prompt, output_key="entities")
summary_chain = LLMChain(llm=llm, prompt=summary_prompt, output_key="summary")
```

---

### Designing for Conditional Prompting Logic

Certain tasks require prompts to vary depending on input values or user intent. While LangChain does not support full branching within a single prompt, developers can simulate conditional logic by:

1. Structuring prompts with embedded conditions

2. Preprocessing input to select which prompt to use

3. Creating nodes in LangGraph for explicit branching

**Pattern Example: Include optional constraint if available**

```python
template = """
Task: {task}
{#if constraint}
Please ensure to follow this rule: {constraint}
{#endif}
Provide a step-by-step response.
"""
```

While not supported natively, such conditional formatting can be implemented manually in code before template construction.

---

### Few-Shot Prompting

Few-shot prompting improves stability by including sample inputs and outputs. This can be embedded directly in the template.

```python
template="""
Q: What is the capital of France?
A: Paris

Q: Who wrote Hamlet?
A: William Shakespeare

Q: {question}
A:
"""
```

This establishes a response pattern for the LLM.