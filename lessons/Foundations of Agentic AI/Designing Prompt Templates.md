
## Understanding Prompts in Agentic Systems

This lesson includes a hands-on coding exercise. After reviewing the material below, please open the lab assignment using the link below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Foundations of Agentic AI/Creating_and_Using_Prompt_Templates.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

---

Prompts are how developers communicate with large language models (LLMs). In agentic AI, prompts do more than ask questions — they guide agents through reasoning, tool usage, and reporting. A well-crafted prompt transforms an LLM into a planning and execution engine.

Prompt templates are structured strings with variable placeholders. They allow developers to consistently deliver instructions, context, goals, and tool access in a reusable format.

---

## Static vs. Dynamic Prompt Templates

LangChain supports two types of prompt templates:

- **Static Templates**: Fixed instructions used repeatedly for similar tasks. Simple but not flexible.
- **Dynamic Templates**: Adjust to user input, task type, or agent state. Variables are added at runtime for personalized prompting.

### Example: Dynamic Template

```python
from langchain.prompts import PromptTemplate

prompt = PromptTemplate(
    input_variables=["task"],
    template="You are an assistant. Follow the steps to complete the task: {task}"
)
```

Dynamic templates help scale prompt design and reduce hardcoding.

---

## Designing Effective Prompt Templates

A strong prompt template should:

- Define the agent's role  
- Break down reasoning into **Thought**, **Action**, **Observation**  
- Specify when and how to use tools  
- Signal when the task is complete  

### ReAct Pattern Template

```
You are an intelligent agent.

Task: {task}

Use this format:
Thought:
Action:
Action Input:
Observation:
Repeat until task is complete.
```

This format supports structured reasoning and can include fallback steps or memory cues.

---

## ReAct and Plan-Based Strategies

### ReAct (Reasoning and Acting)

Agents alternate between thinking and acting. They reason, use tools, observe results, and reflect. Ideal for tasks needing multiple steps.

#### ReAct Example

```
Thought: I need to fetch the weather for Berlin.
Action: get_weather
Action Input: Berlin
Observation: Sunny, 24°C
```

### Plan-and-Execute

Agents first create a full plan, then execute it step-by-step. Useful for long or complex tasks.

#### Plan-and-Execute Example

```
Plan:
1. Retrieve weather data
2. Summarize temperature
3. Output final answer

Executing step 1...
```

---

## Prompt Engineering with LangChain

LangChain provides tools like `PromptTemplate` and `ChatPromptTemplate` to manage prompts.

### ChatPromptTemplate Example

```python
from langchain.prompts import ChatPromptTemplate

chat_prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("human", "{user_input}")
])
```

These templates can include tool instructions, memory states, and safety checks.