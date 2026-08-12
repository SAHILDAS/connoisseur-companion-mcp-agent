# connoisseur-companion-mcp-agent
End-to-end MCP (Model Context Protocol) application with a custom MCP server, MCP client, LangChain ReAct agent, IBM watsonx, and Gradio chat interface.

# Connoisseur Companion – MCP & Agentic AI Application

An end-to-end **Model Context Protocol (MCP) application** that combines a custom MCP server, MCP client, and a Gradio-powered AI host application using **IBM watsonx**, **LangChain**, and a **ReAct agent loop**.

## Features

* **MCP Server**

  * Restaurant search by name
  * Vibe-based restaurant recommendations
  * Detailed review retrieval
  * Resource endpoints for culinary data

* **MCP Client**

  * Tool discovery and verification
  * Resource discovery
  * Filesystem roots callback
  * LLM sampling callback

* **AI Host Application**

  * Runtime MCP tool discovery
  * ReAct agent loop
  * Tool calling through MCP
  * Gradio chat interface
  * Quick-start restaurant prompts

## Tech Stack

* Python
* FastMCP / Model Context Protocol
* LangChain
* IBM watsonx.ai
* Gradio
* AsyncIO
* JSON / REST-style data handling

## Architecture

User → Gradio UI → LangChain ReAct Agent → MCP Client → MCP Server → Restaurant Data

## Example Queries

* Find me a moody restaurant in DTLA
* Tell me about Iron & Embers
* Show me a detailed review for Sakura Garden

## Key Learning Outcomes

* Built a complete MCP server-client-host architecture
* Implemented runtime tool discovery
* Integrated LLM tool calling with a ReAct loop
* Developed an interactive AI application using Gradio



## Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/connoisseur-companion-mcp-agent.git
cd connoisseur-companion-mcp-agent
```

### 2. Create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate      # Linux/macOS
# .venv\\Scripts\\activate     # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

## Environment Variables

The host application uses IBM watsonx.ai.

Create a `.env` file (or export environment variables) with:

```env
WATSONX_APIKEY=your_api_key
WATSONX_PROJECT_ID=your_project_id
```

## Running the MCP Server

```bash
python server.py
```

## Running the MCP Client

```bash
python client.py
```

## Running the Full AI Host Application

```bash
gradio app.py
```

After launch, Gradio will display:

```
Running on local URL: http://127.0.0.1:7860
Running on public URL: https://xxxxxxxx.gradio.live
```

Open the public URL in your browser and try prompts such as:

* Find me a moody restaurant in DTLA
* Tell me about Iron & Embers
* Show me a detailed review for Sakura Garden
