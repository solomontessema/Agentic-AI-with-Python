## 4.1: Connecting to SQLite in Python

https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Define_schema_extraction_logic.ipynb

Why Use SQLite in Agentic Systems

SQLite is a lightweight, serverless SQL engine that requires no external configuration. It is ideal for prototyping AI agents that need to reason over structured data. In this unit, the reader will build the foundation for enabling GPT to understand and query a real database.

Agents that query databases must first understand the schema—what tables exist, what columns they contain, and how they relate. This contextual data will later be passed into GPT to guide SQL generation.

Python includes native support for SQLite via the sqlite3 module.

Basic connection example:


import sqlite3

# Connect to (or create) a database file
db = sqlite3.connect("data.db")
cursor = db.cursor()

# Create a sample table
cursor.execute("""
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT,
    signup_date TEXT
);
""")

db.commit()

Exploring Table Schema

To support GPT’s reasoning, we must extract the database schema and convert it into text that can be used in prompts.

Retrieve table names:


cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
tables = cursor.fetchall()
print("Tables:", tables)

Describe table columns:


cursor.execute("PRAGMA table_info(users);")
columns = cursor.fetchall()
print("Columns:", columns)

These operations will form the foundation of a reusable schema inspection tool.