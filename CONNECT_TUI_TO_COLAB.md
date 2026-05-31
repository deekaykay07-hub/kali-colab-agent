# How to Connect Your Kali TUI to the Colab Backend

Once your Colab notebook is running and you have a public ngrok URL, follow these steps to make your Textual TUI use the powerful remote model.

## Option 1: Quick & Dirty (Environment Variable)

When starting your TUI, set the `OLLAMA_HOST` environment variable:

```bash
OLLAMA_HOST=https://your-ngrok-url.ngrok-free.app python3 -m app.main
```

This makes the `ollama` Python library talk to your Colab instance instead of local Ollama.

## Option 2: Modify the TUI Code (Recommended Long-Term)

Edit `app/main.py` in your TUI repo and change the Ollama client initialization to support a remote base URL.

Example:

```python
import os

OLLAMA_HOST = os.getenv("OLLAMA_HOST", "http://localhost:11434")

# Then when calling:
# ollama.chat(..., host=OLLAMA_HOST)  # or configure client globally
```

We will add proper remote backend support in this repo soon.

## Important Notes

- The ngrok URL changes every time you restart the Colab notebook.
- You must keep the Colab notebook running (do not disconnect the runtime).
- For production use, consider a paid ngrok plan or a proper cloud GPU for stability.

## Next

We will also update the bridge so it can receive commands from the Colab-powered TUI.
