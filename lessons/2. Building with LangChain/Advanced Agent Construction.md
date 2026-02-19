## 2.4: Advanced Agent Construction

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Building with LangChain/Building_a_Conversational_Agent.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/MN28rgMgUnTcwy5xBCPph" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---

LangChain supports building intelligent agents that use reasoning to select tools, maintain memory, and interact over multiple steps. These agents are constructed using the create_agent() function, which enables the integration of a language model, tools, system prompt, middleware, and checkpointing to create structured, persistent conversational systems.
Agents differ from chains and runnables in that they determine their control flow dynamically. They inspect each interaction, select appropriate tools, and respond accordingly—often over extended conversations. This section guides the reader through the process of building an agent by combining key LangChain components in a composable manner.

---

### Defining Tools for the Agent
As it is discussed earlier, tools are functional components the agent can call. Each tool is registered with a name, function, and description. For example, a tool to check product inventory might look like:

```python
def check_inventory(product_name: str) -> str:
    return f"There are 10 count of {product_name}."

# tool definition
check_inventory_tool = Tool(
    name="check_inventory",
    func=check_inventory,
    description="Returns the current stock count for a given  product name."
)
```

---

### Designing the System Prompt
The system prompt defines the agent's behavior and objectives. It is passed directly into the create_agent() configuration and influences how the language model interprets user input and decides on actions.
A system prompt might include structured instructions:

```python
system_prompt = """
You are a helpful and detailed customer service agent.
You must use your tools whenever necessary to answer questions about inventory.
"""
```

System prompts can include stepwise guidance, tool usage policies, or domain constraints to shape the agent's reasoning process.

---

### Adding Summarization Middleware
Middleware enables runtime control of interaction, such as summarizing conversations to reduce memory usage. One common middleware is SummarizationMiddleware, which triggers based on token count or message length:

```python
from langchain.agents.middleware import SummarizationMiddleware

summarization_middleware = SummarizationMiddleware(
     model="gpt-4o-mini",
     trigger=('tokens', 1000),
     keep=('messages', 5),
     )
```

This middleware preserves the most recent exchanges and summarizes earlier messages, keeping the agent within token constraints.

---

### Creating the Agent

With the language model, tools, system prompt, middleware, and checkpointing defined, the agent can be assembled using create_agent():

```python
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

conversational_agent = create_agent(
    model=llm,
    tools=[check_inventory_tool],
    system_prompt=system_prompt,
    middleware=[summarization_middleware],
    checkpointer=InMemorySaver(),
    )

```
The agent object returned by create_agent() manages the full loop of message processing, reasoning, tool invocation, and state management.
