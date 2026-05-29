# Mastering Hello World with MCP: Build Your First AI "USB-C" Port in 5 Minutes

This guide provides a step-by-step approach to building your first Model Context Protocol (MCP) server, enabling your AI to interact with the real world securely.

## Summary

Imagine if every time you bought a new electronic device, you had to buy a brand-new, proprietary power cable. That was the state of AI integration—until **Model Context Protocol (MCP)** arrived. MCP acts like the **USB-C port for AI models**. It provides a universal standard to connect Large Language Models (LLMs) to your local files, databases, and APIs without rewriting integration logic for every different AI provider.

In this guide, we will build a "Hello World" MCP server that allows an AI assistant to fetch real-time (mocked) stock data for symbols like **SPY** or **AAPL**.

📣 This guide is part of the [SkillHunt User Guides](https://skillhunt.codrixtech.com/) series — your go-to collection of practical, beginner-friendly tutorials.

## 🧠 Core Concepts: The MCP Architecture

- **MCP Host:** The AI application (like Claude Desktop or an IDE) that wants to use the tool.
- **MCP Server:** A small program (which we will write) that exposes specific tools or data.
- **Tools:** The actual functions the AI can call (e.g., `get_stock_price`).

## ⚙️ Setting Up Your Environment

We will use **Python 3.11+** and the `FastMCP` framework, which is the quickest way to get a server running.

1. **Create a project directory:**
   ```bash
   mkdir mcp-hello-world && cd mcp-hello-world
   ```

2. **Set up a virtual environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install the MCP SDK:**
   ```bash
   pip install mcp
   ```

## 💻 Writing the MCP Server

Create a file named `server.py`. We will build a tool that validates a stock symbol and returns its current price. This aligns with our standard practice of using financial utility examples.

```python
from mcp.server.fastmcp import FastMCP
import random

# 1. Initialize FastMCP Server
mcp = FastMCP("TechSpark-Finance-Tool")

@mcp.tool()
def get_stock_price(symbol: str) -> str:
    """
    Retrieves the current market price for a given stock symbol.
    """
    symbol = symbol.upper()
    # Simulating a price lookup for our favorite ETF: SPY
    mock_prices = {
        "SPY": 520.45,
        "AAPL": 189.30,
        "MSFT": 415.22,
        "NVDA": 890.11
    }
    
    price = mock_prices.get(symbol, round(random.uniform(10, 1000), 2))
    return f"The current price of {symbol} is ${price}"

if __name__ == "__main__":
    mcp.run()
```

## 🚀 Running and Testing

To see your "USB-C" port in action, run the server using the MCP inspector, which provides a web interface to test your tools:

```bash
mcp dev server.py
```

✅ **Validation:** Once the inspector opens, type `SPY` into the `symbol` argument of the `get_stock_price` tool and click "Run Tool." You should see the mocked price returned immediately!

## 💡 Pro Tips for MCP Development

- **Be Descriptive:** The docstring inside your `@mcp.tool()` function is what the AI uses to understand *when* to call the tool. Make it clear!
- **Statelessness:** Keep your MCP tools stateless. The AI should provide the necessary context in the arguments.
- **Security:** Never hardcode API keys. Use environment variables within your `venv`.

---
*Built with ❤️ by Codrix Technologies. Part of the SkillHunt Knowledge Base.*