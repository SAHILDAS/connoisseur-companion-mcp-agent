# Connoisseur Companion — MCP Host Application

An intelligent AI restaurant assistant that combines a **Model Context Protocol (MCP) server**, **MCP client**, **IBM WatsonX LLM**, and **Gradio** into a complete end-to-end application.

Connoisseur Companion can discover restaurant tools at runtime, reason about which tool to use, call MCP tools through a ReAct (Reason + Act) agent loop, and present natural-language restaurant recommendations through a modern chat interface.

---

## Overview

This project demonstrates the complete MCP workflow:

1. **MCP Server** exposes California restaurant data and search tools.
2. **MCP Client** connects to the server, discovers tools/resources, and communicates over stdio.
3. **MCP Host Application** integrates a WatsonX LLM with the discovered MCP tools and serves a Gradio chat interface.

The result is a production-style AI assistant capable of answering restaurant questions by dynamically invoking MCP tools rather than relying on hardcoded function calls.

---

## Features

- **Runtime MCP tool discovery**
- **ReAct agent loop (Reason + Act)**
- **IBM WatsonX Granite LLM integration**
- **Gradio chat interface**
- **Restaurant lookup by name**
- **Restaurant recommendations by vibe or atmosphere**
- **Detailed restaurant review retrieval**
- **Streaming “Thinking…” UI placeholder**
- **Public Gradio sharing support**

---

## Architecture

```text
+----------------------+
|     Gradio UI        |
|  (Connoisseur        |
|   Companion)         |
+----------+-----------+
           |
           v
+----------------------+
|   WatsonX Granite    |
|   ReAct Agent Loop   |
+----------+-----------+
           |
           v
+----------------------+
|   MCP Host (app.py)  |
|  Discovers MCP Tools |
+----------+-----------+
           |
     stdio / MCP
           |
           v
+----------------------+
|  MCP Server          |
|  (server.py)         |
+----------+-----------+
           |
           v
+----------------------+
| California Restaurant|
| Dataset (TXT + JSON) |
+----------------------+
```

---

## Project Structure

```text
connoisseur-companion/
├── app.py                          # Gradio + WatsonX MCP host
├── server.py                       # MCP server exposing tools/resources
├── client.py                       # MCP client implementation
├── test.py                         # MCP server test script
├── California-Culinary-Map.txt     # Raw restaurant descriptions
├── structured-restaurant-data.json # Structured restaurant database
├── augmented-user-review.json      # Augmented restaurant reviews
├── screenshots/                    # Lab screenshots (optional)
└── README.md
```

---

## MCP Components

### MCP Resource

| URI | Description |
|-----|-------------|
| `culinary-map://california` | Full California culinary map text |

### MCP Tools

| Tool | Purpose |
|------|---------|
| `get_restaurant_info` | Look up a restaurant by name |
| `recommend_by_vibe` | Find restaurants matching a mood or atmosphere |
| `get_review` | Retrieve a detailed restaurant review |

---

## Prerequisites

- Python 3.11+
- IBM WatsonX Project
- WatsonX API credentials configured
- Virtual environment support

---

## Installation

### Clone the repository

```bash
git clone https://github.com/yourusername/connoisseur-companion.git
cd connoisseur-companion
```

### Create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate
```

Windows:

```powershell
.venv\\Scripts\\activate
```

### Install dependencies

```bash
pip install gradio==6.9.0 fastmcp==3.1.0 langchain-ibm==1.0.4 langchain-core==1.2.18
```

---

## Environment Variables

Configure your WatsonX credentials before launching the application.

```bash
export WATSONX_APIKEY="your_api_key"
export WATSONX_AI_PROJECT_ID="your_project_id"
```

Windows:

```powershell
set WATSONX_APIKEY=your_api_key
set WATSONX_AI_PROJECT_ID=your_project_id
```

---

## Running the MCP Server

Start the MCP server:

```bash
python server.py
```

The server exposes the restaurant tools and resource over the MCP stdio transport.

---

## Testing the MCP Server

Run:

```bash
python test.py
```

Expected output:

```json
{
  "status": "found",
  "count": 1,
  "results": [
    {
      "name": "Iron & Embers",
      "cuisine": "American steakhouse",
      "rating": 4.8
    }
  ]
}
```

This verifies that the MCP server is functioning correctly.

---

## Running the MCP Client

Execute:

```bash
python client.py
```

The client will:

- Connect to the MCP server
- Discover tools
- Discover resources
- Verify configured roots
- Demonstrate all three MCP tool calls

---

## Running the Full Application

Launch the complete MCP host application:

```bash
gradio app.py
```

Example terminal output:

```text
Starting Connoisseur Companion...
Running on local URL:  http://127.0.0.1:7860
Running on public URL: https://xxxxxxxx.gradio.live
```

Open the **public URL** in your browser to interact with the application.

---

## Example Queries

### Restaurant lookup

```text
Tell me about Iron & Embers
```

### Vibe recommendation

```text
Find me some moody restaurants in DTLA
```

### Zen dining

```text
What’s a zen dining experience in Little Tokyo?
```

---

## ReAct Agent Workflow

The application uses a ReAct loop:

1. User asks a question.
2. LLM reasons about which tool is needed.
3. MCP tool is called.
4. Tool output is returned.
5. LLM synthesizes a natural-language answer.
6. Final response is shown in the chat UI.

Pseudo-flow:

```text
User
  |
  v
LLM
  |
  +--> get_restaurant_info
  |
  +--> recommend_by_vibe
  |
  +--> get_review
  |
  v
Final Answer
```

---

## Quick-Start Buttons

The Gradio interface includes three built-in prompts:

- **Find moody restaurants**
- **Tell me about Iron & Embers**
- **Zen dining in Little Tokyo?**

These demonstrate automatic MCP tool selection without requiring manual input.

---

## Screenshots

### Main Application

Add your UI screenshot here.

```text
screenshots/M4L3_Design_LLM_MCP_Host.jpg
```

### MCP Server Test

Add your server JSON screenshot here.

```text
screenshots/M4L1_Configure_Tools_Data_MCP_Server.jpg
```

### MCP Client Verification

Add your client discovery screenshot here.

```text
screenshots/M4L2_Build_Test_MCP_Client.jpg
```

---

## Technologies Used

- **Python 3.11**
- **FastMCP 3.1.0**
- **Model Context Protocol (MCP)**
- **IBM WatsonX**
- **Granite LLM**
- **LangChain**
- **Gradio**
- **AsyncIO**

---

## Troubleshooting

### Connection closed

Ensure:

- `server.py` exists in the project directory.
- JSON data files are present.
- The virtual environment is activated.
- `fastmcp==3.1.0` is installed.

### WatsonX authentication error

Verify:

```bash
echo $WATSONX_AI_PROJECT_ID
echo $WATSONX_APIKEY
```

### Gradio public URL missing

Run:

```bash
python app.py
```

or wait a few seconds for the share link to initialize.

---

## Future Improvements

- Conversation memory
- Multi-tool planning
- Geolocation-based restaurant search
- Cuisine filtering
- Reservation integration
- Vector database semantic retrieval
- Streaming token responses
- Docker deployment
- Hugging Face Spaces deployment

---

## Learning Outcomes

This project demonstrates:

- Building an MCP server
- Exposing resources and tools through MCP
- Creating an MCP client
- Runtime tool discovery
- Roots and sampling concepts
- Integrating WatsonX with MCP
- Implementing a ReAct agent loop
- Building an AI-powered Gradio application

---

## License

This project was created as part of an IBM MCP / AI Agent capstone laboratory exercise and is intended for educational purposes.

---

## Acknowledgements

- IBM Skills Network
- IBM WatsonX
- FastMCP
- LangChain
- Gradio

---

**Connoisseur Companion** — Your AI guide to California’s restaurant scene.
