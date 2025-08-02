

# UI Wireframes – MacAgent

These wireframes illustrate the core layout and interaction flow for MacAgent’s floating menu bar interface. The goal is to maintain a minimal UI footprint, only displaying what's needed for input and response confirmation.

---

## 1. Default State (Idle, Awaiting Input)

```plaintext
┌──────────────────────────────────────────────┐
│ MacAgent ▪ AI Assistant                      │
├──────────────────────────────────────────────┤
│ [🔍 Ask me anything..._____________________] │
│                                              │
│ (No active query)                            │
└──────────────────────────────────────────────┘
```

- Floating window appears on keyboard shortcut
- Input field auto-focuses
- No output or task shown yet

---

## 2. Response Streaming

```plaintext
┌──────────────────────────────────────────────┐
│ MacAgent ▪ AI Assistant                      │
├──────────────────────────────────────────────┤
│ [You: Schedule a reminder for 3 PM]          │
│                                              │
│ Assistant:                                   │
│ ✔️ Scheduled a reminder at 3 PM.             │
│ [Confirm]   [Cancel]                         │
└──────────────────────────────────────────────┘
```

- Shows user input and streaming assistant response
- Offers confirmation for data-level task execution

---

## 3. Voice Input Mode

```plaintext
┌──────────────────────────────────────────────┐
│ MacAgent ▪ Voice Listening... 🎙️             │
├──────────────────────────────────────────────┤
│ (Waveform animation here)                    │
│ Listening... (say your command)              │
└──────────────────────────────────────────────┘
```

- Triggered by a different keyboard shortcut
- Audio is transcribed and passed to the same input processor

---

## 4. Tool Invocation and System Context

```plaintext
┌──────────────────────────────────────────────┐
│ MacAgent ▪ Screen Analysis                   │
├──────────────────────────────────────────────┤
│ Agent detected:                              │
│ - App: Mail                                  │
│ - Subject: “Lunch plans for Thursday”        │
│ What would you like to do?                  │
│ [Summarize]  [Set reminder]  [Ignore]        │
└──────────────────────────────────────────────┘
```

- Used when the agent analyzes screen content
- Proposes context-aware options

---

## Design Notes

- The floating window should be no more than 400px wide
- Rounded corners and slight drop shadow
- Uses system font (San Francisco) and native styling
- UI disappears only when dismissed or focus returns to prior app