# Architecture – Google Colab Powered Kali Wireless Agent

## High-Level Vision

Run a strong coding + planning model (7B–13B class) on Google Colab T4 GPU, while keeping the ability to execute real commands on the user's local machine (especially for wireless hardware).

The interface stays as a clean, fast Textual TUI (same feel as the current VPS version or Gemini CLI).

## Recommended Architecture

```
[User Browser]
      |
      v
[Textual TUI]  <--- served via ttyd or a simple web server
      |
      |  (talks to remote model via OpenAI-compatible API)
      v
[Google Colab T4]  (runs Ollama or llama.cpp with GPU)
      ^
      |
      |  (optional: tool calls / function calling)
[Local Bridge]  (runs on user's laptop)
      |
      v
[User's Hardware]  (WiFi monitor mode, Bluetooth, NFC, etc.)
```

## Components

### 1. Google Colab Notebook (the brain)
- Runs a strong model (qwen2.5-coder:7b, deepseek-coder:6.7b, etc.)
- Uses GPU acceleration (T4)
- Exposes an API (Ollama, OpenAI compatible, or raw HTTP)
- Uses ngrok (or similar) to get a public URL
- Can optionally run some Kali tools inside Colab (for non-hardware tasks)

### 2. TUI Client
- Textual app (same style as current kali-mistral-tui)
- Can run locally on the user's machine or on a cheap VPS
- Talks to the Colab backend for completions
- Handles special commands (`/bridge-token`, planning panel, etc.)

### 3. Local Bridge (already started in sibling repo)
- Small agent that connects to the TUI
- Executes commands on the user's actual hardware
- Streams output back

## Benefits
- Much faster and smarter models than VPS CPU
- Still have real hardware access via the bridge
- Cheap / free GPU power
- Familiar terminal UI

## Challenges
- Colab sessions time out after a few hours (need to re-run notebook)
- Need stable tunnel (ngrok is easiest)
- Bridge + TUI connection must be resilient to Colab restarts

## Next Steps
- [ ] Create first Colab notebook with Ollama + GPU + ngrok
- [ ] Make the TUI able to talk to a remote OpenAI-compatible endpoint
- [ ] Improve bridge authentication and reconnection logic
- [ ] Add model switching from the TUI
