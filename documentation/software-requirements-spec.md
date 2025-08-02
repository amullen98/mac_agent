


# Software Requirements Specification (SRS) – MacAgent

## 1. Introduction

### 1.1 Purpose
This document outlines the functional and non-functional requirements for MacAgent, a macOS AI assistant designed to help users execute tasks via natural language. It defines what the app will do, how it will behave, and the scope of its initial implementation for the week-of-code MVP.

### 1.2 Scope
MacAgent is a floating, always-on-top AI assistant for macOS. It will live in the menu bar and be triggered by keyboard shortcuts. The app will send user queries to an OpenAI Assistant, process streamed responses, and perform actions such as local system commands or summarizing what's on screen. It will support both text and voice input.

### 1.3 Intended Audience
This document is intended for the developer(s), contributors, and reviewers of the project.

---

## 2. Functional Requirements

### 2.1 Floating UI
- The app will appear as a small, always-on-top window anchored to the top corner of the screen.
- It will also be accessible via a menu bar icon.

### 2.2 Text Input
- When triggered, the input field should instantly gain focus.
- The user can type natural language commands into this field.
- Pressing `Enter` submits the query to the AI backend.

### 2.3 Voice Input
- A second global shortcut will trigger voice input.
- Captured speech is transcribed and sent to the AI agent.

### 2.4 Assistant Integration
- Uses OpenAI Assistants API (GPT-4o or similar) with persistent thread memory.
- Can call custom tools such as `run_shell_command`, `schedule_reminder`, etc.

### 2.5 Tool Execution
- Tools defined in the assistant config will be handled locally by the app.
- The agent should perform tasks at the system or data level whenever possible, only surfacing results in the UI for user confirmation or review, rather than automating UI interactions.

### 2.6 Streaming Output
- The agent will display streaming responses in the floating window.
- The UI should update incrementally as output is received.

### 2.7 Screen Context Awareness
- The agent can request a screenshot or OCR scan of the current screen.
- The result will be sent to the agent for context-based response generation.

---

## 3. Non-Functional Requirements

### 3.1 Performance
- The agent should respond within 1–2 seconds of receiving a command.
- Voice input must convert to text with minimal delay.

### 3.2 Platform Compatibility
- Must work on macOS 13.0+ (preferably native on Apple Silicon).
- Swift + SwiftUI or AppKit permitted.

### 3.3 Privacy & Permissions
- Must request and handle macOS permissions gracefully:
  - Microphone
  - Screen recording
  - Accessibility (if tool execution requires it)

### 3.4 Security
- No sensitive data will be stored.
- API keys or secrets must be secured using the system keychain.

---

## 4. Out of Scope for MVP
- Cloud syncing of memory or settings
- Advanced customization UI
- User onboarding or tutorial flows
- Full accessibility support

---

## 5. Future Enhancements
- Natural language scheduling
- App integrations (e.g., Mail, Calendar, Notes)
- Theme customization
- History log or chat view
