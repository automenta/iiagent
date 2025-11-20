<div align="center">
  <img src="assets/ii.png" width="200"/>

# II Agent

[![GitHub stars](https://img.shields.io/github/stars/Intelligent-Internet/ii-agent?style=social)](https://github.com/Intelligent-Internet/ii-agent/stargazers)
[![Discord Follow](https://dcbadge.limes.pink/api/server/yDWPsshPHB?style=flat)](https://discord.gg/yDWPsshPHB)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Blog](https://img.shields.io/badge/Blog-II--Agent-blue)](https://ii.inc/web/blog/post/ii-agent)
[![GAIA Benchmark](https://img.shields.io/badge/GAIA-Benchmark-green)](https://ii-agent-gaia.ii.inc/)
[<img src="https://devin.ai/assets/deepwiki-badge.png" alt="Ask DeepWiki.com" height="20"/>](https://deepwiki.com/Intelligent-Internet/ii-agent)

</div>

II-Agent is an open-source intelligent assistant designed to streamline and enhance workflows across multiple domains. It represents a significant advancement in how we interact with technology—shifting from passive tools to intelligent systems capable of independently executing complex tasks.

II-Agent Chat also feature within II-Agent that lets you work across multiple models and tools in one place. The web-hosted version comes with Gemini 3, Sonnet 4.5, and GPT-5 ready to use out of the box. You can connect your own API keys, switch models within a single thread, and use them alongside Claude Skills, GPT-5 Code Interpreter, and text file search.


### Discord Join US

📢 Join Our [Discord Channel](https://discord.gg/yDWPsshPHB)! Looking forward to seeing you there! 🎉

### Try now on our web application version at [II-Agent](https://agent.ii.inc/)

## Introduction

<https://github.com/user-attachments/assets/2707b106-f37d-41a8-beff-8802b1c9b186>

## Key Features:
* Full-Stack Development: Complete web app scaffolding and iterative development. From initial setup to deployment, II-Agent handles the entire development lifecycle with intelligent code generation and optimization.
* Slide Creation: Transform short briefs into polished presentations. Create professional slides and decks with intelligent content structuring, design suggestions, and automated formatting.
* Deep Research: Comprehensive research capabilities through tight integration with II-Researcher. Conduct thorough investigations, analyze data, and generate detailed reports with our specialized research agent.

## SWE-Bench Pro

<img width="1778" height="1060" alt="swepro" src="https://github.com/user-attachments/assets/e955538d-986a-4c74-96b9-dbeb56e803e1" />



## Installation

For the latest installation and deployment instructions, please refer to our [official guide](https://intelligent-internet.github.io/ii-agent-prod/)

[![Installation Guide](https://img.youtube.com/vi/wPpeJMbdGi4/maxresdefault.jpg)](https://www.youtube.com/watch?v=wPpeJMbdGi4)

## Development Roadmap

### Completed Migration Phases

#### Phase 3: User Experience & State Management
- [x] **Session Management API**: Implement REST endpoints (`GET`, `PATCH`, `DELETE`) for Sessions in `core/src/routes/sessions.ts`.
- [x] **Chat History API**: Implement `GET /sessions/:id/events` to allow the frontend to load previous conversation history.
- [x] **User Settings**: Implement `core/src/routes/settings.ts` to allow users to save and retrieve their LLM API keys and Model preferences.

#### Phase 4: Advanced Agent Capabilities
- [x] **Code Execution Tool**: Implement a `code_interpreter` tool using `quickjs-emscripten`.
- [x] **MCP (Model Context Protocol) Client**: Implement an MCP Client in `core/src/mcp/` to allow the Agent to connect to external tools.
- [x] **Web Browser Tool**: Port the web browsing capabilities using `playwright`.

#### Phase 5: Integrations & Polish
- [x] **Connectors Framework**: Create a basic structure for external connectors (Google Drive, etc.).
- [x] **Billing/Stripe**: Port the Stripe integration (mock/stub implemented).
- [x] **Final Verification**: Full end-to-end testing of the Agent loop with all tools and frontend integration (implemented in `core/tests/agent_e2e.test.ts`).

### Phase 6: Advanced Context Engineering & Architecture

We are implementing a sophisticated context management system to handle long-running, complex tasks without losing state or incurring massive token costs.

#### 1. Slab Checkpoints & Microkernels
Instead of simple context eviction (dropping old messages), we will implement **Slab Checkpoints**.
*   **Threshold Detection**: Checkpoints are triggered based on model-specific context window usage (e.g., 90% for 64K models, 15% for 1MB models).
*   **Microkernel Generation**: Before a checkpoint is created, the system generates a "Microkernel"—a concise summary of active tasks, goals, current activity, and important "breadcrumbs" (file references, key decisions).
*   **Non-Eviction**: The original context remains in the "L1" (active) memory until explicitly managed, while the checkpoint is stored (potentially as QR-encoded video "MemVid" for extreme compression) and indexed.

#### 2. Context Modes
The agent will support distinct modes of operation to optimize for different cognitive loads:
*   **SUSPENDED**: The agent operates solely on the Microkernel. Useful for low-cost monitoring or waiting states.
*   **HIGH_DETAIL**: The agent expands specific historical contexts using recall tools. It retrieves "Slabs" relevant to the current focus (e.g., "OAuth implementation") to regain deep situational awareness.
*   **HIGH_CAPACITY**: The context is stripped to the bare minimum (Microkernel only) to free up maximum token budget for complex reasoning or architectural planning.
*   **NORMAL**: Standard operation with the active context window.

#### 3. Tool-Driven Recall
The LLM is not a passive recipient of context but an active manager. It will have access to specific tools:
*   `RecallContext(query)`: Allows the agent to search historical checkpoints for breadcrumbs related to a specific topic.
*   `MicrocontextSubroutine(query, depth)`: Temporarily expands the context with digested information from past slabs, allowing the agent to perform a "deep dive" without permanently cluttering the active window.

#### 4. REPL & Lite Installation
To support diverse developer needs, we are introducing:
*   **REPL Mode**: An interactive Command Line Interface (CLI) with tab completion, ideal for quick tasks and local development without a browser.
*   **Lite Installation**: A modular installation option that includes only the core dependencies (LLM providers, basic tools) (~200MB), separating heavy dependencies like Google Cloud Platform SDKs and full server infrastructure (~2GB) into optional extras.
