


# Technical Design Document – MacAgent

This document outlines the technical details for implementing the core functionality of the MacAgent AI assistant app. It breaks down the architecture into implementable components and highlights key patterns, APIs, and system interactions.

---

## 1. Application Framework

- **Language**: Swift
- **UI Framework**: SwiftUI (with AppKit bridge where needed)
- **Target Platform**: macOS 13+ (Apple Silicon preferred)
- **Deployment Style**: Menu bar app with persistent floating panel

---

## 2. Global Shortcut Handling

### Objective
Allow the user to trigger the app from anywhere using:
- One shortcut to open and focus the input window
- Another shortcut to initiate voice capture

### Implementation
Use the [`HotKey`](https://github.com/soffes/HotKey) library to register global keyboard shortcuts:
```swift
let hotKey = HotKey(key: .a, modifiers: [.command, .option])
hotKey.keyDownHandler = {
    appController.toggleFocus()
}
```

Store and toggle the `previousApp` using `NSWorkspace.shared.frontmostApplication` to restore context after a second shortcut press.

---

## 3. Floating Input UI

### Objective
Display a small, always-on-top window anchored in the top-right (or configurable) corner of the screen.

### Implementation
- Use `NSWindow` set to `.floating` level
- `NSPanel` or SwiftUI-based `NSWindowRepresentable` for popover-style view
- Auto-focus the input field:
```swift
textField.becomeFirstResponder()
```

The input UI listens for `Enter` to submit and `Esc` to dismiss.

---

## 4. Voice Input

### Objective
Convert spoken language to text and submit to the AI agent

### Implementation
- Use `AVAudioEngine` + `SFSpeechRecognizer`
- Initiate capture on shortcut trigger
- Update UI with waveform animation while listening
- On speech end, send transcribed text to AI

**Permissions Required**:
- `NSMicrophoneUsageDescription` in `Info.plist`

---

## 5. Assistants API Integration

### Objective
Send user messages to the OpenAI Assistants API and process response/tool call flow.

### Implementation
- Use `URLSession` or async HTTP client to POST:
  - `POST /v1/threads/{id}/messages`
  - `POST /v1/threads/{id}/runs`
- Enable streaming response with `stream: true`
- Persist thread ID for each user session in local file or UserDefaults

Tool calls returned will be passed to the Tool Execution Layer.

---

## 6. Tool Execution Layer

### Objective
Respond to function calls from the assistant with local logic

### Tool Examples
- `run_shell_command`
- `create_reminder`
- `analyze_screen`

### Implementation
Use a dispatcher system to map tool names to Swift closures:
```swift
toolRegistry["run_shell_command"] = { args in
    shellRunner.execute(args["command"])
}
```

Avoid UI automation. Perform tasks via Apple APIs or shell when possible.

---

## 7. Streaming Response Display

### Objective
Update the UI incrementally as the response is received from the Assistants API

### Implementation
- Use `URLSessionStreamTask` or manual `URLSession` with chunk decoding
- Append each chunk to the response window in real-time
- Scroll-to-bottom animation or follow cursor position

---

## 8. Screen Capture + OCR

### Objective
Enable the agent to “see” what’s on screen to support context-based actions

### Implementation
- Use `CGWindowListCreateImage` to capture the frontmost window or screen
- Use VisionKit or integrate Tesseract for OCR
- Send OCR'd text and metadata to the AI agent via a tool call (e.g., `analyze_screen`)

**Permissions Required**:
- `NSCameraUsageDescription`
- `NSScreenCaptureAccess`

---

## 9. Security and Permissions

- Use macOS’s sandboxing where possible
- Store API keys in Keychain
- Prompt for required permissions with instructions if blocked
- Avoid capturing or logging sensitive data unless explicitly approved

---

## 10. Error Handling and Fallbacks

- Gracefully handle network timeouts or API failures
- Show unobtrusive error state in the floating window
- Provide default response when tools fail (e.g., “Couldn’t complete, try again”)
