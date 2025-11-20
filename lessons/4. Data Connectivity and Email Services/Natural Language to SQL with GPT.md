5.2: Natural Language to SQL with GPT

https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Natural_Language_to_SQL_Generator.ipynb

From Questions to Queries: GPT as a SQL Generator

Converting natural language questions into SQL queries is one of the most powerful applications of LLMs in data-driven agents. Given the schema of a database, GPT can reason about which tables and fields to access, which filters to apply, and how to construct valid SQL syntax.

The reliability of this process depends on prompt clarity, schema accuracy, and error handling.

Prompt Engineering for SQL Generation

To convert a natural question into a valid SQL query, the prompt must:

Describe the schema

Set a clear instruction to generate SQL

Possibly request explanations or justification

Basic prompt structure:

Given the following database schema:
{schema}

Write an SQL query to answer this question:
{question}

Implementation:


from langchain.prompts import PromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.chains import LLMChain

schema_prompt = PromptTemplate(
    input_variables=["schema", "question"],
    template="""
    You are a data assistant. Given the database schema:

    {schema}

    Write a syntactically correct SQLite SQL query to answer the question:
    {question}
    """
)

llm = ChatOpenAI(model="gpt-4", temperature=0)
sql_chain = LLMChain(llm=llm, prompt=schema_prompt)
