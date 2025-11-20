## 3.3: Stateful and Modular Graph Components

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Task Flow with LangGraph/Composing_Modular_Nodes.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/owYQuARVuoi8bnwTwFknjdPN" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---
### Shared State in LangGraph

LangGraph maintains a shared mutable dictionary across the graph. Nodes can:

- Read prior outputs

- Add new fields

- Update or overwrite values

This enables flexible, context-aware workflows.

---

### State Management Tips

- Use TypedDict to define structure

- Prefix keys to avoid collisions

- Avoid overwriting unless intentional

- Log transitions for debugging

Example:

```python
class MyGraphState(TypedDict):
    input: str
    summary_result: str
    keyword_result: str
    metadata: dict
```

### Modular Node Design

Modular nodes improve clarity, scalability, and reusability. Each node should:

- Operate on a single field or transformation

- Use clear, descriptive names

- Be testable independently of the graph

Example: 1.

```python
def summarize_node(state):
    from langchain.prompts import PromptTemplate
    from langchain.chains import LLMChain
    from langchain.chat_models import ChatOpenAI
    from dotenv import load_dotenv
    load_dotenv()


    llm = ChatOpenAI(model="gpt-4", temperature=0)
    prompt = PromptTemplate(
        input_variables=["input"],
        template="Summarize the following: {input}"
    )
    chain = LLMChain(llm=llm, prompt=prompt)

    summary = chain.run(state["input"])
    state["summary_result"] = summary
    return state
```

This node can be reused in any graph with an "input"

Example 2.

```python
def keyword_node(state):
    from langchain.prompts import PromptTemplate
    from langchain.chains import LLMChain
    from langchain.chat_models import ChatOpenAI
    from dotenv import load_dotenv
    load_dotenv()

    llm = ChatOpenAI(model="gpt-4", temperature=0)
    prompt = PromptTemplate(
        input_variables=["input"],
        template="Extract the most relevant keywords from the following text. Return them as a comma-separated list:\n\n{input}"
    )
    chain = LLMChain(llm=llm, prompt=prompt)

    keywords = chain.run(state["input"])
    state["keyword_result"] = keywords
    return state
```