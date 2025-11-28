## Tool Integration in LangChain

---

This lesson includes a hands-on coding exercise. After reviewing the material below, please open the lab assignment using the link below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Foundations of Agentic AI/Agent_with_Multiple_Tools.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

---

### Tools as Functional Interfaces for Agents

In agentic AI, tools serve as the agent's hands. While the LLM provides reasoning and language understanding, tools allow it to interact with external systems, fetch data, transform inputs, or perform specialized tasks.

A tool in LangChain is a callable object (usually a Python function) wrapped with metadata: a name, a description, and optional argument schema. Tools expand the agent’s capabilities beyond natural language.

Agents reason about when and how to use tools based on:
* Tool descriptions
* Reasoning patterns in the prompt
* Observed outputs from prior tool use

---

### Tool Definition in LangChain

A typical LangChain tool includes:
* A Python function for the core logic
* A wrapper using the Tool class
* A descriptive string that helps the LLM understand when to use it

Example:

```python
from langchain_core.tools import Tool

def simple_math(expression: str) -> str:
    try:
        result = eval(expression)
        return str(result)
    except Exception:
        return "Invalid mathematical expression."

math_tool = Tool(
    name="simple_math",
    func=simple_math,
    description="Useful for calculating simple math."
)
```

This math_tool can be added to any agent’s toolset.

---

### Tool Metadata and Selection

Agents decide which tool to use based on the description field of the tool object. Well-written descriptions significantly improve tool selection accuracy.

Best Practices for Descriptions:
* Include task scope: "summarize text", "send email"
* Use clear keywords for the LLM to match to intent
* Keep it concise but informative

Agents don’t understand code—they understand tool metadata through natural language reasoning.

---

### Registering Tools with Agents

To enable an agent to use tools:
1.	Define the tools in a tools/ module
2.	Import them in agents/base_agent.py
3.	Register them during agent initialization

Example:
```python
from langchain.agents import create_agent

agent = create_agent(
    model=llm,
    tools= [time_tool, math_tool],
    system_prompt="You are a helpful assistant. You must use the provided tools"
)
```

---

### Handling Tool Errors and Edge Cases

Since tools execute arbitrary Python code, they must be resilient to:
* Missing inputs
* Network failures
* Unexpected results or empty returns

Safe Tool Patterns:
* Add input validation
* Wrap logic in try-except blocks
* Return helpful error messages for debugging

Example:
```python
def simple_math(expression: str) -> str:
    try:
        result = eval(expression)
        return str(result)
    except Exception:
        return "Invalid mathematical expression."
```
