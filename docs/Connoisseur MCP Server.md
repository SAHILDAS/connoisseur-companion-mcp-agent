# Connoisseur MCP Server

A **FastMCP-based Model Context Protocol (MCP) server** that exposes California restaurant and culinary data through standardized MCP **resources** and **tools**. This project converts previously built restaurant search logic into a reusable protocol-compliant service that any MCP-compatible client or AI agent can discover and invoke.

## Overview

This server wraps a California culinary dataset and exposes it through MCP so that external agents can:

- Read the full California Culinary Map as a resource.
- Search restaurants by name.
- Get restaurant recommendations based on a vibe or atmosphere keyword.
- Retrieve detailed augmented review records.

The implementation uses **FastMCP 3.1.0** and demonstrates the transition from standalone Python scripts to interoperable MCP services.

## Features

### MCP Resource

- **`culinary-map://california`** — returns the complete raw California Culinary Map text.

### MCP Tools

- **`get_restaurant_info(restaurant_name)`** — search restaurants by full or partial name.
- **`recommend_by_vibe(vibe)`** — recommend restaurants using structured vibe tags and raw text search.
- **`get_review(restaurant_name)`** — retrieve the full augmented review for a restaurant.

## Project Structure

```text
.
├── server.py                         # FastMCP server implementation
├── test.py                           # MCP client test script
├── California-Culinary-Map.txt       # Raw culinary map text
├── structured-restaurant-data.json   # Structured restaurant dataset
├── augmented-user-review.json        # Augmented review dataset
├── .venv/                            # Python virtual environment
└── README.md
```

## Prerequisites

- Python 3.11+
- `pip`
- Linux/macOS shell (or Windows PowerShell equivalent)

## Setup Instructions

### 1. Create a virtual environment

```bash
pip install virtualenv
virtualenv .venv
source .venv/bin/activate
```

### 2. Install dependencies

Install FastMCP:

```bash
pip install fastmcp==3.1.0
```

### 3. Download the data files

#### California Culinary Map

```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/_nbA_KMj1n7yBrpfz8rYkg/California-Culinary-Map.txt
```

#### Structured Restaurant Data

```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/lxfhTQUrDCCD_JSMmr92VA/structured-restaurant-data.json
```

#### Augmented User Reviews

```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/oMqDIzTBNFT7KKJ0GW4-Cw/augmented-user-review.json
```

### 4. Create the server file

```bash
touch server.py
```

## Server Implementation

### Initialize FastMCP

```python
from fastmcp import FastMCP
from pathlib import Path
import json

mcp = FastMCP("Connoisseur-Server")
```

### Load datasets

```python
DATA_DIR = Path(__file__).parent
CULINARY_MAP_PATH = DATA_DIR / "California-Culinary-Map.txt"
RESTAURANT_DATA_PATH = DATA_DIR / "structured-restaurant-data.json"
REVIEW_DATA_PATH = DATA_DIR / "augmented-user-review.json"

def load_restaurant_data():
    with open(RESTAURANT_DATA_PATH, "r") as f:
        return json.load(f)

def load_review_data():
    with open(REVIEW_DATA_PATH, "r") as f:
        return json.load(f)
```

### Expose the California Culinary Map as an MCP Resource

```python
@mcp.resource("culinary-map://california")
def get_culinary_map():
    return CULINARY_MAP_PATH.read_text()
```

### Tool 1: Restaurant Lookup

```python
@mcp.tool()
def get_restaurant_info(restaurant_name: str):
    ...
```

Searches the structured restaurant dataset and returns JSON-formatted restaurant information.

### Tool 2: Recommend by Vibe

```python
@mcp.tool()
def recommend_by_vibe(vibe: str):
    ...
```

Performs a two-pass search:

1. Structured vibe tags in the JSON dataset.
2. Raw text descriptions in the California Culinary Map.

### Tool 3: Retrieve Reviews

```python
@mcp.tool()
def get_review(restaurant_name: str):
    ...
```

Returns a full augmented review record including reviewer, rating, review text, image description, and visit date.

### Server Entry Point

```python
if __name__ == "__main__":
    mcp.run()
```

## Running the MCP Server

Start the server:

```bash
python server.py
```

Expected startup output:

```text
FastMCP 3.1.0
Server: Connoisseur-Server
Starting MCP server 'Connoisseur-Server' with transport 'stdio'
```

## Testing the Server

Use the provided `test.py` script:

```bash
python test.py
```

The script connects to the MCP server through **stdio transport**, initializes a client session, and calls the `get_restaurant_info` tool.

### Example Output

```text
--- START SCREENSHOT ---
{
  "status": "found",
  "count": 1,
  "results": [
    {
      "name": "Iron & Embers",
      "neighborhood": "Arts District, DTLA",
      "cuisine": "American steakhouse",
      "type": "fine dining",
      "rating": 4.8,
      "price_range": "$$$$",
      "signature_dish": "45-day dry-aged ribeye with bone marrow chimichurri",
      "vibes": [
        "moody",
        "industrial"
      ],
      "description": "A space that smells deeply of white oak smoke and masculine sophistication."
    }
  ]
}
--- END SCREENSHOT ---
```

## Example MCP Tool Calls

### Restaurant Search

Input:

```json
{
  "restaurant_name": "Iron"
}
```

Output:

```json
{
  "status": "found",
  "count": 1,
  "results": [
    {
      "name": "Iron & Embers",
      "rating": 4.8,
      "price_range": "$$$$"
    }
  ]
}
```

### Vibe Recommendation

Input:

```json
{
  "vibe": "romantic"
}
```

Returns matching restaurants and relevant raw text excerpts from the culinary map.

### Review Lookup

Input:

```json
{
  "restaurant_name": "Iron & Embers"
}
```

Returns the full augmented review record.

## MCP Architecture

```text
+-------------------+
|    MCP Client     |
| (test.py / Agent) |
+---------+---------+
          |
          | stdio transport
          |
+---------v---------+
| FastMCP Server    |
| Connoisseur-Server|
+---------+---------+
          |
          |
+---------v---------+
| Resources         |
| - culinary-map    |
+-------------------+

+-------------------+
| Tools             |
| - get_restaurant_info |
| - recommend_by_vibe   |
| - get_review          |
+-------------------+

+-------------------+
| JSON Datasets     |
| - restaurants     |
| - reviews         |
| - culinary map    |
+-------------------+
```

## Troubleshooting

### `Connection closed`

Ensure:

- `server.py` ends with:

```python
if __name__ == "__main__":
    mcp.run()
```

- All data files are present.
- FastMCP version is `3.1.0`.

### Verify syntax

```bash
python -m py_compile server.py
```

### Verify import

```bash
python -c "import server; print(server.mcp)"
```

Expected:

```text
FastMCP('Connoisseur-Server')
```

## Technologies Used

- **FastMCP 3.1.0**
- **Python 3.11**
- **Model Context Protocol (MCP)**
- **JSON**
- **Pathlib**

## Learning Outcomes

This project demonstrates how to:

- Build an MCP server with FastMCP.
- Expose static data as MCP resources.
- Register callable functions as MCP tools.
- Serve structured restaurant data through a standardized protocol.
- Connect an MCP client and invoke tools over stdio transport.

## License

This project was created for educational purposes as part of an MCP and FastMCP learning workflow.