# Browsing Agent

This repository contains an AI agent that uses the `google-adk` library to control a headless Chrome browser. It can perform browsing-related tasks by programmatically interacting with web pages.

## How it Works

The agent is built using the `google-adk` (Agent Development Kit). It uses `chrome-devtools-mcp` (Machine-Control Plane) to instrument and control a Chrome browser instance.

The core of the agent is defined in `agent/agent.py`. It is configured with a detailed prompt that guides it on how to interact with web pages by taking snapshots, evaluating scripts, and performing actions like clicking and filling fields.

## Getting Started

### Prerequisites

- You need to have `npx` available on your system, which is part of Node.js.
- Python 3.7+

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/example/browsing-agent.git
    cd browsing-agent
    ```

2.  **Install the required Python packages:**
    ```bash
    pip install -r agent/requirements.txt
    ```

### Usage

The agent can be used as a library or integrated into a larger application. Here is a basic example of how to instantiate and run the agent:

```python
from agent.agent import root_agent

# The root_agent is pre-configured and ready to use.
# You can now send tasks to the agent.
# For example, to ask it to search for something on Google:
response = root_agent.invoke({"task": "Search for 'AI Agents' on Google and tell me the text of the first result."})

print(response)
```

## Agent Prompting Guide

The agent is designed to follow a specific workflow for interacting with web pages. Here is a summary of its core directives:

1.  **`take_snapshot()`**: Get a structured representation of the page to understand its layout and get unique identifiers (`uid`) for interactive elements.
2.  **Analyze and Extract**: Review the snapshot to find the `uid` of the target element.
3.  **Execute Action**: Use the `uid` with an action tool like `click(uid=...)`, `fill(uid=...)`, etc.
4.  **Verify**: Confirm the result of your action, usually by taking another snapshot or using `evaluate_script` to check for changes.

### Key Principles:

-   **Always start with `take_snapshot()`**: This is the foundation for any interaction.
-   **Use `evaluate_script` for precision**: Extract specific text or values from elements without parsing the full snapshot.
-   **Never guess a `uid`**: Only use `uid`s obtained from a `take_snapshot` call.
-   **Handle dynamic content**: Use `wait_for(text=...)` to ensure elements have loaded.
-   **Manage tabs**: Use `list_pages` and `select_page` when working with multiple tabs.
-   **Recover from errors**: If a tool call fails, take a new snapshot to reassess the page's state.