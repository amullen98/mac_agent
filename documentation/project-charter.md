

# Project Charter: AI Agent Mac App

## Project Name
**MacAgent** – A persistent, always-on AI assistant accessible from the macOS menu bar.

## Purpose
To build a lightweight, customizable AI agent app for macOS that enhances user productivity by enabling natural language task execution through a floating menu bar interface. The agent supports both text and voice input, interacts with screen content, and can remember previous interactions to provide contextual assistance.

## Objectives
- Create a small, always-on-top menu bar app interface
- Implement a keyboard shortcut to toggle input focus
- Support voice input through a secondary shortcut
- Maintain persistent memory using OpenAI’s Assistants API
- Execute tool-based tasks (e.g., scripts, system actions) via function calling
- Enable the agent to “see” the user’s screen for context-aware actions

## Scope
This project will focus on building a working proof-of-concept during a one-week development sprint. It will not include production-ready error handling, user settings, or full deployment packaging.

## Stakeholders
- **Developer/Owner:** Andrew Mullen
- **End Users:** macOS users who want a streamlined, always-accessible AI assistant
- **Technology Stack:** Swift/SwiftUI, OpenAI API, macOS native permissions and frameworks

## Deliverables
- Functional menu bar agent with floating UI
- Keyboard and voice input triggers
- Integration with OpenAI Assistants API (with tool calling and memory)
- Basic screen reading (OCR or screenshot-based)
- Demo-ready version by the end of the coding sprint

## Success Criteria
- The agent launches and responds to text input
- The shortcut toggles focus correctly
- Voice input captures and sends text to the agent
- The agent returns and displays streamed responses
- One or more tool calls (e.g., “open Notes”, “schedule reminder”) execute successfully
- The agent can describe or summarize a screenshot of the current screen

## Constraints
- Development time limited to one week
- macOS permissions (screen, mic, accessibility) may block some functionality unless granted manually
- MVP only – polish, settings, and extensibility deferred