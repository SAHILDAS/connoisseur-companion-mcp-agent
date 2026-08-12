# Connoisseur Client — MCP Client Application

An MCP (Model Context Protocol) client that connects to the **Connoisseur MCP Server**, discovers available tools and resources at runtime, declares filesystem roots, handles delegated LLM sampling requests, and demonstrates all server tools through the MCP protocol.

This project represents the **client side of the MCP architecture**, completing the communication layer between AI applications and external tools.

---

## Overview

The MCP Client launches the MCP server as a subprocess and communicates with it over **stdio** using the official MCP Python SDK.

It demonstrates:

- Establishing an MCP session
- Discovering server tools
- Discovering server resources
- Declaring filesystem roots
- Handling delegated LLM sampling requests
- Calling all server tools through the MCP protocol

---

## Architecture

```text
+----------------------+
|    MCP Client        |
|    (client.py)       |
+----------+-----------+
           |
      stdio / MCP
           |
           v
+----------------------+
|    MCP Server        |
|    (server.py)       |
+----------+-----------+
           |
           v
+----------------------+
| Restaurant Dataset   |
|  - Culinary Map TXT  |
|  - Structured JSON   |
|  - Review JSON       |
+----------------------+
```

The client launches `server.py` automatically using `StdioServerParameters` and communicates with it through `ClientSession`.

---

## Features

- **Automatic MCP server launch**
- **Tool discovery at runtime**
- **Resource discovery**
- **Filesystem roots callback**
- **Anthropic sampling callback**
- **JSON tool response parsing**
- **Reusable session helper**
- **Three demonstration functions**
- **Connection verification and validation**

---

## Project Structure

```text
connoisseur-client/
├── client.py                         # MCP client implementation
├── server.py                         # MCP server from previous lab
├── California-Culinary-Map.txt       # Raw culinary dataset
├── structured-restaurant-data.json   # Structured restaurant database
├── augmented-user-review.json        # Restaurant reviews
├── screenshots/                      # Optional lab screenshots
└── README.md
```

---

## MCP Server Dependencies

This client requires the MCP server created in the previous lab.

Required files:

- `server.py`
- `California-Culinary-Map.txt`
- `structured-restaurant-data.json`
- `augmented-user-review.json`

---

## Server Capabilities

### Resources

| URI | Description |
|-----|-------------|
| `culinary-map://california` | Full California culinary map text |

### Tools

| Tool | Purpose |
|------|---------|
| `get_restaurant_info` | Look up restaurants by name |
| `recommend_by_vibe` | Find restaurants by mood or atmosphere |
| `get_review` | Retrieve detailed restaurant reviews |

---

## Prerequisites

- Python 3.11+
- Virtual environment
- Anthropic API access (for sampling callback)
- MCP server from the previous lab

---

## Installation

### Clone the repository

```bash
git clone https://github.com/yourusername/connoisseur-client.git
cd connoisseur-client
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
pip install fastmcp==3.1.0 anthropic==0.84.0 mcp==1.25.0
```

---

## Environment Variables

Configure your Anthropic API key:

```bash
export ANTHROPIC_API_KEY="your_api_key"
```

Windows:

```powershell
set ANTHROPIC_API_KEY=your_api_key
```

---

## Client Configuration

The client automatically locates the server script and project directory.

```python
SERVER_SCRIPT = str(Path(__file__).parent / "server.py")
PROJECT_DIR = Path(__file__).parent.resolve()
```

This ensures the client works regardless of where the project folder is located.

---

## Filesystem Roots

The client declares which directories the server is allowed to access.

```python
def list_roots() -> list[Root]:
    return [Root(uri=f"file://{PROJECT_DIR}", name=PROJECT_DIR.name)]
```

This limits server file access to the current project directory.

---

## Sampling Callback

The client implements an MCP **sampling callback**, allowing the server to delegate LLM requests to the client.

```python
async def handle_sampling(params):
    ...
```

The callback:

1. Receives the prompt from the server
2. Calls Anthropic Claude
3. Returns the generated response
4. Keeps API keys entirely on the client side

---

## Session Helper

All communication happens through a reusable helper.

```python
async def call_tool(tool_name, arguments):
    ...
```

This helper:

- Opens an MCP session
- Registers roots and sampling callbacks
- Initializes the connection
- Calls a tool
- Parses the JSON response
- Returns a Python dictionary

---

## Connection Verification

Before running demos, the client verifies that the server exposes the expected capabilities.

### Tool discovery

```python
tools_result = await session.list_tools()
```

### Resource discovery

```python
resources_result = await session.list_resources()
```

The verification step confirms:

- `get_restaurant_info`
- `recommend_by_vibe`
- `get_review`
- `culinary-map://california`

---

## Demo Functions

### Demo 1: Restaurant Lookup

```python
await demo_get_restaurant_info()
```

Example query:

```text
Iron & Embers
```

Returns structured restaurant details including cuisine, rating, price range, vibes, and description.

---

### Demo 2: Recommend by Vibe

```python
await demo_recommend_by_vibe()
```

Example query:

```text
moody
```

Returns:

- Structured restaurant matches
- Raw culinary map text excerpts

---

### Demo 3: Retrieve Review

```python
await demo_get_review()
```

Example query:

```text
Iron & Embers
```

Returns:

- Reviewer name
- Rating
- Full review text
- Image description
- Visit date

---

## Running the Client

Execute:

```bash
python client.py
```

The client will automatically:

1. Launch `server.py`
2. Verify tools
3. Verify resources
4. Verify roots
5. Execute all three demo functions

---

## Example Output

### Connection Verification

```text
============================================================
MCP Connection Verification
============================================================

Discovered 3 tools:
  - get_restaurant_info
  - recommend_by_vibe
  - get_review

All required tools verified!

Discovered 1 resources:
  - culinary-map://california

Configured 1 roots:
  - project: file:///home/project
```

### Restaurant Lookup

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

---

## MCP Workflow

```text
Client
  |
  | initialize()
  |
  v
Server
  |
  | list_tools()
  |
  v
Client
  |
  | call_tool()
  |
  v
Server
  |
  | execute tool
  |
  v
Client
```

---

## Technologies Used

- **Python 3.11**
- **FastMCP 3.1.0**
- **MCP Python SDK**
- **Anthropic SDK**
- **AsyncIO**
- **JSON**
- **Pathlib**

---

## Screenshots

### MCP Client Verification

Add your lab screenshot here.

```text
screenshots/M4L2_Build_Test_MCP_Client.jpg
```

### MCP Server JSON Output

```text
screenshots/M4L1_Configure_Tools_Data_MCP_Server.jpg
```

---

## Troubleshooting

### Connection closed

Ensure:

- `server.py` exists
- Data files exist
- The virtual environment is active
- `fastmcp==3.1.0` is installed

### Tool not found

Run the verification step first.

Expected tools:

- `get_restaurant_info`
- `recommend_by_vibe`
- `get_review`

### Anthropic authentication error

Verify:

```bash
echo $ANTHROPIC_API_KEY
```

---

## Future Improvements

- Persistent session reuse
- Automatic retry logic
- Streaming tool responses
- Multi-server support
- Parallel tool execution
- Conversation memory
- Tool caching
- Structured logging

---

## Learning Outcomes

This project demonstrates:

- Building an MCP client
- Connecting to an MCP server over stdio
- Runtime tool discovery
- Runtime resource discovery
- Roots callbacks
- Sampling callbacks
- MCP session management
- JSON-RPC tool invocation
- Asynchronous Python programming

---

## License

This project was created as part of an IBM MCP / AI Agent capstone laboratory exercise and is intended for educational purposes.

---

## Acknowledgements

- IBM Skills Network
- Model Context Protocol (MCP)
- FastMCP
- Anthropic
- Python AsyncIO

---

**Connoisseur Client** — A complete MCP client that discovers, verifies, and invokes restaurant tools through the Model Context Protocol.
