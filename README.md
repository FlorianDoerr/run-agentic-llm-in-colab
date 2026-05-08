# Run LLM in Colab

This notebook sets up an [Ollama](https://ollama.com/) runtime inside a Linux notebook environment such as Google Colab, downloads a model, warms it up in memory, and exposes the Ollama API through a Cloudflare tunnel so it can be reached from outside the notebook.

## How to use

1. Run the setup cell.
2. Wait for the model download to finish.
3. Copy the printed Cloudflare URL to access the Ollama endpoint remotely.

## Notes

- Connect to a T4 runtime.
- If you want a different model, change the `MODEL` value in the notebook before running the setup cell.
- If you run out of RAM, for example on a low-RAM CPU runtime or when using a larger model, decrease the context length.
- For agentic coding, you can use any VS Code extension (e.g., Cline) that allows you to run Ollama from a URL.
- Small models like `qwen3.5:9b` are sufficient for documentation, spell-checking, and minor coding tasks. Do not expect the model to perform like a large frontier model.

## What the notebook does

The notebook performs these steps:

1. Updates the system package index and installs required Linux tools such as `pciutils` and `zstd`.
2. Installs Ollama from the official install script.
3. Configures Ollama environment variables:
   - `OLLAMA_CONTEXT_LENGTH=128000`
   - `OLLAMA_HOST=0.0.0.0`
   - `OLLAMA_KEEP_ALIVE=-1`
4. Starts the Ollama server in the background.
5. Pulls the selected model, currently `qwen3.5:9b`.
6. Runs a small prompt once to load the model into RAM.
7. Downloads `cloudflared`, creates a tunnel to `http://localhost:11434`, and prints the public `trycloudflare.com` URL.
