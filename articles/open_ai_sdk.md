# Getting Started with OpenAI’s Agents SDK

## 🤔 What is the Agents SDK (and why should you care)?

The [**OpenAI Agents SDK**](https://openai.github.io/openai-agents-python/) is a lightweight framework from OpenAI for building agentic apps: apps where a **model follows instructions, calls tools, hands off to other agents, and respects guardrails—without** you writing a ton of scaffolding. 

Think “production‑ready Swarm successor” with a tiny set of primitives:

- **Agent** 🧠 (the brain of your app that interprets and responds)
- **Tools** 🛠️ (functions or hosted capabilities the agent can call)
- **Runner** 🏃 (executes your agent and manages the interaction loop)
- **Handoffs** 🔄 (pass control or context to another agent)
- **Guardrails** 🚧 (rules to validate or restrict behavior)
- **Tracing** 📜 (logs and insights into what the agent did and why)

### 🔍 How it differs from the old Assistants API

- **Scope & Design:** Assistants was a monolith for threads/runs/files/tools. The new stack uses **Responses + Agents SDK** for cleaner orchestration and tool use.
- **Lifecycle:** OpenAI announced the **Responses API** and **Agents SDK** as the path forward, with plans to phase out Assistants by mid‑2026. If you’re starting fresh, build with Agents.
- **Developer UX:** Agents SDK gives you native constructs (**handoffs, guardrails, tools**), plus tracing in the dashboard—far less glue code.

**In short:** Assistants was a vertical; **Agents SDK** is a minimal orchestration layer on top of **Responses** (and other providers via LiteLLM), built for multi‑tool, multi‑agent apps.

## 🌐 Popular Agent Frameworks in the Industry

Before diving into tool calling, it's worth noting that OpenAI’s Agents SDK is part of a growing ecosystem of agent frameworks. Popular options include:

- **LangChain 🔗** – a comprehensive toolkit for building LLM-powered apps with chains, agents, and integrations.
- **LlamaIndex 📚** – focuses on connecting LLMs to external data via retrieval-augmented generation.
- **Microsoft Semantic Kernel 💠** – integrates AI models, plugins, and orchestration with .NET and Python support.
- **Haystack 🌾** – open-source framework for building search and question-answering systems.
- **CrewAI 🧑‍🤝‍🧑** – coordinates multiple specialized agents working together toward a goal.

These frameworks vary in focus, integrations, and complexity. The Agents SDK stands out for its native integration with OpenAI’s Responses API, built-in guardrails, handoffs, and hosted tools.

## 🛠️ Tool Calling 101 (the mental model)

An **Agent** can “take actions” via **tools**. In the Agents SDK you get three classes:

- **Hosted tools** 🌍 (Web Search, File/Docs Retrieval, Computer Use, Code Interpreter, etc.).
- **Function tools** ⚙️ (your Python functions, automatically schema‑ized from signatures + docstrings).
- **Agents as tools** 🤝 (let one agent call another without a full handoff).

Under the hood, this is OpenAI’s modern **tool calling** capability (formerly “function calling”)—the model decides if and how to call your tool, with structured arguments.

## ⚙️ Setup (Python)

### Create a project and virtual environment

```bash
mkdir tspark-hello-openai-agent
cd tspark-hello-openai-agent
python -m venv .venv
```

_💡 Note_: New to virtual environments? No worries! 🎉 Check out our beginner-friendly guide [Mastering Python Virtual Environments: A Beginner’s Guide](https://skillhunt.codrixtech.com/guide/view?src=python_venv.md&id=541ae60f-5fbf-487f-bf1d-c681923afb37) for a fun, step-by-step walkthrough that gets you set up like a pro.

### Activate the virtual environment

Do this every time you start a new terminal session.

```bash
source .venv/bin/activate
```

### Install the Agents SDK

```bash
pip install openai-agents
```

### Set an OpenAI API key

If you don't have one, follow [these instructions](https://platform.openai.com/docs/quickstart#create-and-export-an-api-key) to create an OpenAI API key.

```bash
export OPENAI_API_KEY=sk-...
```

## 👋 “Hello, World” Agent + One Custom Tool

This minimal example shows:

- an **Agent** with concise instructions,
- one **custom function tool** using the `@function_tool` decorator,
- a single turn run via `Runner.run.`

```python
# hello_agents.py
import asyncio
from datetime import datetime, timezone

from agents import Agent, Runner, function_tool

@function_tool
def what_time(is_utc: bool = True) -> str:
    """
    Return the current time as a formatted string.

    Args:
        is_utc: If true, return time in UTC; otherwise use local time.
    """
    now = datetime.now(timezone.utc) if is_utc else datetime.now()
    return now.strftime("%Y-%m-%d %H:%M:%S %Z")

# Define your agent
greeter = Agent(
    name="Greeter",
    instructions=(
        "You are a friendly assistant. "
        "Greet the user and, if they ask for the time, call the 'what_time' tool."
    ),
    tools=[what_time],  # register our custom tool
)

async def main():
    # Try a prompt that nudges tool use
    result = await Runner.run(greeter, "Hi there! What's the time right now?")
    print("\n--- Final Output ---")
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

▶️ Run it:

```bash
python hello_agents.py
```

### 🔍 What’s happening

- `@function_tool` turns a plain Python function into a structured tool: the SDK infers the JSON schema from the signature and docstring, so the model knows how to call it safely.
- The model decides when to call what_time, the SDK executes the function, and the final message includes your function’s output.

## 🚀 Next steps (quick pointers)

- **Use hosted tools:** web search, file search (vector stores), computer use, code interpreter—no extra boilerplate.
- **Add guardrails & tracing:** validate inputs/outputs and inspect every run in the dashboard trace viewer.
- **Compose agents:** model “specialists” and wire them via handoffs or “agents as tools” for orchestration.
- **Go realtime/voice:** the Agents SDK pairs with the Realtime API for low‑latency, voice agents.

**💬 Note for SkillHunt Readers:**

We publish practical, beginner-friendly guides like this to help you level up your full-stack development skills. Keep exploring our [SkillHunt User Guides](https://skillhunt.codrixtech.com/) for more hands-on tutorials. 🚀