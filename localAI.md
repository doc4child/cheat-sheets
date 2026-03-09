# Ollama

Install ollama first.

download model and chat with it. 

gpt-oss:20b

# Install Claude Code terminal version

```

curl -fsSL https://claude.ai/install.sh | bash

```

Setup notes:
  • Native installation exists but ~/.local/bin is not in your PATH. Run:

  ```

  echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc

```
run 
"claude"

login using API (google main personal account since CC is provided)


https://www.youtube.com/watch?v=jJ9jPzPdyDg

using it with openrouter

https://www.youtube.com/watch?v=p4KD56w2kpc&t=152s

From terminal use these commands to tell claude code to use this oss model.

```


export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="token"
claude --model gpt-oss:20b

```
