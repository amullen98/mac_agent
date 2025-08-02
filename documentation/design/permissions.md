


# Permissions – MacAgent

This document outlines all macOS permissions required for MacAgent to function correctly, including what each permission enables, how it is requested, and any user setup that may be required.

---

## 1. Microphone Access

**Why it's needed:**
- Enables voice input via speech-to-text for user commands.

**How it's handled:**
- Add the following to `Info.plist`:
  ```xml
  <key>NSMicrophoneUsageDescription</key>
  <string>MacAgent uses your microphone to capture voice commands.</string>
  ```
- The system will automatically prompt the user on first use.

---

## 2. Screen Recording

**Why it's needed:**
- Allows the app to take screenshots for screen analysis and OCR.

**How it's handled:**
- macOS requires manual user approval via:
  > System Settings → Privacy & Security → Screen Recording → Enable MacAgent
- This access must be explicitly granted by the user; your app can display an onboarding prompt or help message when access is missing.

---

## 3. Accessibility Access (Optional)

**Why it's needed:**
- Required only if MacAgent needs to perform any UI-based system interaction (e.g. clicking buttons, switching apps).
- May be used for edge cases or fallback behavior.

**How to check:**
- Use:
  ```swift
  AXIsProcessTrusted()
  ```

**How to request:**
- Direct user to:
  > System Settings → Privacy & Security → Accessibility → Enable MacAgent

---

## 4. Network Access

**Why it's needed:**
- Required for communicating with OpenAI's Assistants API and streaming responses.

**How it's handled:**
- Network access is typically granted by default unless restricted by firewall/proxy settings.

---

## 5. Keychain Access

**Why it's needed:**
- Secure storage for OpenAI API keys or other user secrets.

**How it's handled:**
- Use Keychain APIs (`KeychainAccess` or `Security.framework`) to store and retrieve secrets securely.
- No system prompt is shown, but it's important to note that sensitive data is not stored in plain text.

---

## Summary Table

| Permission         | Required | Purpose                                  | User Action Needed          |
|--------------------|----------|------------------------------------------|-----------------------------|
| Microphone         | ✅       | Voice input                              | Prompted automatically      |
| Screen Recording   | ✅       | Screenshot + OCR                         | Manual enable in Settings   |
| Accessibility      | ⬜️       | UI interaction fallback (optional)       | Manual enable in Settings   |
| Network Access     | ✅       | API calls to OpenAI                      | Automatic (unless restricted)|
| Keychain Access    | ✅       | Store API keys securely                  | No prompt shown             |