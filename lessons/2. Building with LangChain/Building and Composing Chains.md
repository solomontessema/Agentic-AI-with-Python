## 2.2: Building and Composing Chains

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Building with LangChain/Implementing_a_Runnable_Sequence.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/ZrG2Spx4QaphoRgeuKfwY" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---

### The Role of Chains in LangChain

Chains in LangChain serve as the fundamental orchestration mechanism for guiding language model behavior through structured, multi-step tasks. Unlike agents, which determine control flow at runtime, chains are deterministic pipelines where each step is predefined and executed in sequence.

Chains are ideal for scenarios requiring controlled reasoning, sequential formatting, and repeated application of logic—such as:

- Summarizing data  
- Formatting outputs  
- Transforming inputs  
- Chaining multiple LLM calls  

LangChain supports several chain variants:

- **LLMChain** — A basic chain that pairs a single prompt with a language model.  
- **SimpleSequentialChain** — A linear chain where the output of one step becomes the input of the next.  
- **SequentialChain** — An advanced chain that allows named inputs/outputs and intermediate variable passing.  

---

## Creating an LLMChain for Question Answering

The simplest chain in LangChain is the **LLMChain**, which takes a prompt and an LLM and returns a response. It is well-suited for atomic tasks like Q&A, summarization, or classification.

```python
llm = ChatOpenAI(model="gpt-4", temperature=0)

prompt = PromptTemplate(
    input_variables=["topic"],
    template="What are the latest trends in {topic}?"
)

chain = LLMChain(llm=llm, prompt=prompt)
response = chain.run({"topic": "machine learning"})
print(response)
```

---
### Composing Multi-Step Logic with SimpleSequentialChain

SimpleSequentialChain allows developers to stack multiple LLMChain objects in a pipeline. The output of one chain becomes the input of the next. This is useful when reasoning and formatting occur in stages.

#### Example: Summarize → Rephrase

```python
from langchain.chains import SimpleSequentialChain

# First Chain: Summarize input text
summarize_prompt = PromptTemplate(
    input_variables=["input_text"],
    template="Summarize this content: {input_text}"
)
summarize_chain = LLMChain(llm=llm, prompt=summarize_prompt)

# Second Chain: Rephrase the summary
rephrase_prompt = PromptTemplate(
    input_variables=["text"],
    template="Rephrase this summary professionally: {text}"
)
rephrase_chain = LLMChain(llm=llm, prompt=rephrase_prompt)

# Compose the chain
pipeline = SimpleSequentialChain(chains=[summarize_chain, rephrase_chain], verbose=True)

output = pipeline.run("The global AI market is projected to grow significantly over the next decade.")
print(output)
```
This model demonstrates how output from one reasoning stage can be transformed and refined in subsequent steps.

---

### Advanced Composition with SequentialChain

SequentialChain introduces greater control by letting developers name inputs and outputs. This enables complex multi-step systems where intermediate outputs are preserved and passed selectively.

#### Use Case: Extract Entities → Summarize → Format Response

```python
from langchain.chains import SequentialChain

# Prompt 1: Extract named entities
entity_prompt = PromptTemplate(
    input_variables=["input_text"],
    template="Extract all people, organizations, and locations from the following: {input_text}"
)
entity_chain = LLMChain(llm=llm, prompt=entity_prompt, output_key="entities")

# Prompt 2: Summarize entities
summary_prompt = PromptTemplate(
    input_variables=["entities"],
    template="Summarize the following extracted entities: {entities}"
)
summary_chain = LLMChain(llm=llm, prompt=summary_prompt, output_key="summary")

# Prompt 3: Format response
format_prompt = PromptTemplate(
    input_variables=["summary"],
    template="Format this into a user-friendly paragraph: {summary}"
)
format_chain = LLMChain(llm=llm, prompt=format_prompt, output_key="final_output")

# Compose the multi-step chain
sequential_chain = SequentialChain(
    chains=[entity_chain, summary_chain, format_chain],
    input_variables=["input_text"],
    output_variables=["final_output"],
    verbose=True
)

result = sequential_chain({"input_text": "OpenAI and Google DeepMind are competing in developing AGI."})
print(result["final_output"])

```
This modular architecture enables better debugging, logging, and reuse across complex workflows.