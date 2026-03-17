# Tool Use & Function Calling

## Overview

**Tool use** (also called **function calling**) allows LLMs to interact with external systems — APIs, databases, calculators, code interpreters, web browsers — extending their capabilities beyond static knowledge.

Instead of generating a plain text answer, the model can output a structured call to an external tool, receive the result, and incorporate it into its final response.

## Why Tool Use?

LLMs have important limitations that tools can address:

| Limitation | Tool Solution |
|---|---|
| Stale knowledge (training cutoff) | Web search |
| Hallucinated facts/numbers | Database lookup |
| Cannot do precise arithmetic | Calculator / code interpreter |
| Cannot access private data | API calls to internal systems |
| Cannot take real-world actions | Browser, email, file system |

## How Function Calling Works

### Step 1: Define Tools

Provide the model with a schema describing available tools (name, description, parameters):

```json
{
  "name": "get_weather",
  "description": "Get current weather for a city",
  "parameters": {
    "type": "object",
    "properties": {
      "city": {
        "type": "string",
        "description": "City name, e.g. 'London'"
      },
      "units": {
        "type": "string",
        "enum": ["celsius", "fahrenheit"]
      }
    },
    "required": ["city"]
  }
}
```

### Step 2: Model Decides to Call a Tool

When the model determines a tool is needed, it outputs a structured call rather than a text response:

```json
{
  "tool_call": {
    "name": "get_weather",
    "arguments": {
      "city": "London",
      "units": "celsius"
    }
  }
}
```

### Step 3: Execute the Tool

The application intercepts the tool call, executes the function, and gets a result:

```json
{
  "temperature": 14,
  "condition": "Cloudy",
  "humidity": 78
}
```

### Step 4: Model Generates Final Response

The tool result is injected back into the conversation, and the model generates a natural language response:

```
The current weather in London is 14°C and cloudy with 78% humidity.
```

## Parallel Tool Calls

Modern APIs (OpenAI, Anthropic) support multiple simultaneous tool calls in a single turn, allowing the model to batch independent requests:

```json
[
  {"name": "get_weather", "arguments": {"city": "London"}},
  {"name": "get_weather", "arguments": {"city": "Paris"}},
  {"name": "get_weather", "arguments": {"city": "Tokyo"}}
]
```

## Common Built-in Tools

### Code Interpreter
Executes Python code in a sandboxed environment. Enables:
- Mathematical calculations
- Data analysis (pandas, numpy, matplotlib)
- File manipulation
- Chart generation

Available in ChatGPT, Claude's artifacts, Gemini.

### Web Search / Grounding
Retrieves current web content, enabling up-to-date responses.
- OpenAI: `web_search` tool (GPT-4o, o3)
- Anthropic: Via third-party integrations / MCP
- Google: Grounding with Google Search (Gemini)

### File Reading/Writing
Read uploaded documents (PDFs, CSVs, images) and write files.

### Browser / Computer Use
Control a web browser or desktop environment:
- Navigate URLs
- Click buttons
- Fill forms
- Extract page content

## Agentic Frameworks

Tool use enables **agents** — models that autonomously decide which tools to use, in what order, to complete a multi-step goal.

### ReAct Pattern

The most common agent pattern: alternate between **reasoning** and **acting** (tool calls):

```
Thought: I need to find the population of France. I'll search for it.
Action: web_search("France population 2024")
Observation: France population is approximately 68 million (2024)
Thought: I have the answer.
Final Answer: France has approximately 68 million people as of 2024.
```

### Common Agent Frameworks

| Framework | Language | Notes |
|---|---|---|
| **LangChain** | Python/JS | Chains, agents, RAG pipelines |
| **LlamaIndex** | Python | Document-focused RAG and agents |
| **AutoGen** | Python | Multi-agent conversations |
| **CrewAI** | Python | Role-based multi-agent teams |
| **DSPy** | Python | Declarative LM programs |
| **Semantic Kernel** | C#/Python | Microsoft; enterprise focus |
| **Pydantic AI** | Python | Type-safe agent building |

## Model Context Protocol (MCP)

**MCP** (Anthropic, November 2024) is an open protocol that standardizes how AI applications connect to tools and data sources.

- Think of it as "USB-C for AI tools" — a universal connector.
- **MCP servers** expose tools, resources, and prompts.
- **MCP clients** (Claude Desktop, Cursor, IDEs) connect to MCP servers.
- Tools can be: file systems, databases, Git repos, APIs, etc.
- Rapidly adopted across the industry.

## OpenAI Function Calling API

The most widely used tool calling interface:

```python
from openai import OpenAI

client = OpenAI()
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "What's the weather in London?"}],
    tools=[{
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {"type": "string"}
                },
                "required": ["city"]
            }
        }
    }],
    tool_choice="auto"
)
```

`tool_choice` options:
- `"auto"` — Model decides whether to call a tool.
- `"required"` — Model must call a tool.
- `{"type": "function", "function": {"name": "..."}}` — Force a specific function.

## Safety Considerations

Tool use amplifies both LLM capabilities and risks:

- **Prompt injection** — Malicious content in tool results hijacks the agent.
- **Unintended actions** — Agent may take irreversible real-world actions (delete files, send emails).
- **Privilege escalation** — Agent granted more access than needed.
- **Infinite loops** — Agent may loop between tools without making progress.

**Best practices:**
- Principle of least privilege for tool access.
- Require human confirmation for high-impact actions.
- Validate and sanitize all tool inputs and outputs.
- Implement timeouts and step limits.

## See Also

- [Prompting Techniques](prompting.md)
- [Context Windows](context-window.md)
- [ReAct & Agents](prompting.md#react-reason--act)
