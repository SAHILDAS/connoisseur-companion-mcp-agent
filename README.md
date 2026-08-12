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
