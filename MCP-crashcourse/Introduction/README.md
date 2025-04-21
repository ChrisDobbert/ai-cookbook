# Model Context Protocol (MCP) Developer Notebook

This developer reference notebook provides key information about the Model Context Protocol (MCP) for building AI integrations.

## What is MCP?

MCP is an open protocol that standardizes how applications provide context to Large Language Models (LLMs). Think of MCP like a USB-C port for AI applications - it provides a standardized way to connect AI models to different data sources and tools.

## Benefits of MCP

* Access to a growing list of pre-built integrations that your LLM can directly use
* Flexibility to switch between LLM providers and vendors
* Best practices for securing your data within your infrastructure

## Architecture Overview
- **MCP Hosts**: Programs like Claude Desktop, IDEs, or your python application that want to access data through MCP
- **MCP Clients**: Protocol clients that maintain 1:1 connections with servers
- **MCP Servers**: Lightweight programs that each expose specific capabilities through the standardized Model Context Protocol (tools, resources, prompts)
- **Local Data Sources**: Your computer’s files, databases, and services that MCP servers can securely access
- **Remote Services**: External systems available over the internet (e.g., through APIs) that MCP servers can connect to

```mermaid
---
config:
  theme: dark
  layout: dagre
---
flowchart LR
 subgraph Computer["Your Computer"]
        Client["Host with MCP Client<br>(Claude, IDEs, Tools)"]
        ServerA["MCP Server A"]
        ServerB["MCP Server B"]
        ServerC["MCP Server C"]
        DataA[("Local<br>Data Source A")]
        DataB[("Local<br>Data Source B")]
  end
 subgraph Internet["Internet"]
        RemoteC[("Remote<br>Service C")]
  end
    Client -- MCP Protocol --> ServerA & ServerB & ServerC
    ServerA <--> DataA
    ServerB <--> DataB
    ServerC -- Web APIs --> RemoteC
```


## Key Components

* **MCP Hosts**: Programs like Claude Desktop, IDEs, or AI tools that want to access data through MCP
* **MCP Clients**: Protocol clients that maintain 1:1 connections with servers
* **MCP Servers**: Lightweight programs that each expose specific capabilities through the standardized Model Context Protocol
* **Local Data Sources**: Your computer's files, databases, and services that MCP servers can securely access
* **Remote Services**: External systems available over the internet (e.g., through APIs) that MCP servers can connect to

## Core Concepts

<h3><a href="https://modelcontextprotocol.io/docs/concepts/resources">Resources</a></h3>
Resources expose data and content from your servers to LLMs. They allow you to make different types of data sources available to LLMs in a structured way.

<h3><a href="https://modelcontextprotocol.io/docs/concepts/prompts">Prompts</a></h3>
Create reusable prompt templates and workflows for consistent AI interactions across your applications.

<h3><a href="https://modelcontextprotocol.io/docs/concepts/tools">Tools</a></h3>
Enable LLMs to perform actions through your server - from executing code to accessing databases to calling APIs.

<h3><a href="https://modelcontextprotocol.io/docs/concepts/sampling">Tools</a></h3>
Let your servers request completions from LLMs, essentially allowing bidirectional communication between your systems and the AI.

<h3><a href="https://modelcontextprotocol.io/docs/concepts/transports">Transports</a></h3>
The underlying communication mechanisms that connect all MCP components together.

### Transport Mechanisms

MCP supports two main transport mechanisms:

1. **Stdio (Standard IO)**: 
   - Communication occurs over standard input/output streams
   - Best for local integrations when the server and client are on the same machine
   - Simple setup with no network configuration required

2. **SSE (Server-Sent Events)**:
   - Uses HTTP for client-to-server communication and SSE for server-to-client
   - Suitable for remote connections across networks
   - Allows for distributed architectures

Understanding when to use each transport is crucial for building effective MCP implementations:

- Use **Stdio** when building single-application integrations or during development
- Use **SSE** when you need remote accessibility or are integrating with clients that require it

#### Transport Mechanism Comparison

```mermaid
---
config:
  theme: dark
  layout: dagre
---
flowchart LR
 subgraph Stdio["Stdio Transport"]
        Client1["MCP Client"]
        Server1["MCP Server"]
  end
 subgraph SSE["SSE Transport"]
        Client2["MCP Client"]
        Server2["MCP Server"]
  end
 subgraph Local["Local Deployment"]
        Stdio
  end
 subgraph Remote["Remote Deployment"]
        SSE
  end
    Client1 <-- Stdio<br>(Standard I/O)<br>*(bi-directional)* --> Server1
    Client2 -- HTTP POST<br>(client to server) --> Server2
    Server2 -- SSE<br>(server to client) --> Client2
    style Client1 fill:#000000
    style Server1 fill:#000000
    style Client2 fill:#000000
    style Server2 fill:#000000
```
