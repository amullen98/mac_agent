# System Architecture – MacAgent

## Overview

MacAgent is a macOS-native AI assistant that lives in the menu bar and interacts with OpenAI's Assistants API to execute user tasks via natural language. The system is designed to minimize UI automation and instead perform actions at the system or data level, surfacing results to the user only when confirmation is needed.

---

## Architecture Diagram

```plaintext
┌────────────────────┐
│  Menu Bar App (UI) │◄────────────┐
└────────────────────┘             │
        │ Input/Output             │
        ▼                         ▼
┌────────────────────┐     ┌────────────────────┐
│  Input Manager     │     │   Output Renderer  │
│ (text/voice hotkey)│     │ (streaming output) │
└────────────────────┘     └────────────────────┘
        │                         ▲
        ▼                         │
┌────────────────────────────────────────────┐
│       AI Agent Controller (Core Logic)     │
│ Handles sessions, prompts, tool results,   │
│ and responses via Assistants API           │
└────────────────────────────────────────────┘
        │
        ▼
┌────────────────────────┐
│ Assistants API (OpenAI)│
│ + Persistent Threads   │
│ + Tool Calls           │
└────────────────────────┘
        │
        ▼
┌──────────────────────────────┐
│ Tool Execution Layer         │
│ - Shell commands             │
│ - System APIs (e.g. reminders)│
│ - Screen reading + OCR       │
└──────────────────────────────┘
```

---

## Core Components

### 1. Menu Bar App (UI)
- Launches the floating window
- Listens for global keyboard shortcuts
- Displays streaming responses
- Prompts user for confirmation when needed

### 2. Input Manager
- Handles focus behavior (keyboard shortcut → input focus)
- Manages voice capture and transcription

### 3. Output Renderer
- Updates the floating panel with streamed responses
- Displays task summaries or results for user approval

### 4. AI Agent Controller
- Communicates with the Assistants API
- Sends/receives messages
- Intercepts tool call instructions
- Tracks thread IDs to maintain context/memory

### 5. Assistants API (OpenAI)
- Processes user prompts
- Returns responses and function call requests
- Handles memory via persistent threads

### 6. Tool Execution Layer
- Implements locally-defined functions called by the AI
- Executes tasks directly via shell or macOS APIs
- Avoids using the UI unless necessary
- Includes screenshot capture + OCR for context-aware queries

---

## Design Principles

- **Minimize UI Automation:** Prefer native/system-level execution over visual scripting
- **Keep UI Lightweight:** UI should serve as an entry point and review pane, not the core workspace
- **Use Memory:** Maintain threads so the agent can remember past commands
- **Stream Responses:** Give immediate, incremental feedback
- **Respect Privacy:** Request only essential permissions and handle securely

