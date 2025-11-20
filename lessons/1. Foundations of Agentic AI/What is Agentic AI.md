
### **1.1: What is Agentic AI?**

This lesson includes a hands-on coding exercise. After reviewing the material below, please open the lab assignment using the link below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Foundations of Agentic AI/LangChain_Agent_with_Simulated_Tool.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

---

##### Podcast Episode — *What is Agentic AI?*

Learn more about this topic in the accompanying audio lesson.  

<a href="https://copilot.microsoft.com/shares/podcasts/ADAtegkeGP6LavLkPEB9k" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 10px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---

Agentic AI refers to intelligent systems that act autonomously to achieve goals, rather than responding passively to user inputs. These agents reason, use tools, manage memory, and interact over multiple steps.

### Traditional AI vs Agentic AI

| **Aspect**      | **Traditional AI**       | **Agentic AI**                   |
|-----------------|--------------------------|----------------------------------|
| **Behavior**    | Reactive                 | Goal-driven and proactive        |
| **Execution**   | Single-step              | Multi-step and autonomous        |
| **Memory**      | Stateless                | Maintains contextual memory      |
| **Tool Use**    | Predefined or limited    | Dynamic tool selection           |
| **Adaptability**| Fixed logic              | Adaptive to input/ environment   |
| **Interaction** | One-turn response        | Iterative and interactive        |

**Core Features of Agentic AI**

*   **Autonomy**: Acts without constant user input
*   **Memory**: Remembers context across steps
*   **Tool Use**: Interfaces with APIs, databases, services
*   **Reflective Reasoning**: Adjusts plans based on outcomes
*   **Multi-Modality**: Integrates text, data, visuals, and UI

**Enabling Technologies**

**LangChain**

*   Python framework for building agents
*   Includes memory, tools, prompts, decision loops

**LangGraph**

*   Adds control flow to LangChain
*   Models workflows as graphs (nodes = decisions/tools)
*   **GPT API**
*   **Provides reasoning, summarization, and planning**
*   **Powers the agent’s cognitive capabilities**

**Building Intelligent Agents**

Agents consist of:

*   Prompt Templates: Guide reasoning
*   Tool Definitions: Specify operations
*   Memory Modules: Maintain context
*   Agent Executors: Manage control flow

_Example_: An agent tasked with “summarize unread emails and chart frequent senders” would:

*   Use email tools
*   Summarize with GPT
*   Analyze data
*   Create charts

**Levels of Autonomy**

*   Step-Based: Waits for user after each action
*   Loop-Based: Executes steps until task completion
*   Goal-Driven: Decomposes and completes goals independently

LangChain/LangGraph support all modes with:

*   Iteration limits
*   Error handling
*   Human approval checkpoints

**Real-World Applications**

*   Customer Service: Automates queries and escalations
*   Data Analysis: Summarizes and visualizes insights
*   Workflow Automation: Manages emails, records, logs
*   Research Assistance: Reviews literature, extracts findings
