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

First add the export lines in ~/.zshrc file so that they are automatically loaded.

R

Open the file in a text editor:Bash

```
nano ~/.zshrc
```
(or use vim, code, open -e, etc.)
Add these lines at the end of the file:
Bash
```
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="token"
```
→ This is the simplest and most common method for tools like claude (Anthropic's CLI agent) when using Ollama's Anthropic-compatible API.
Save and exit (in nano: Ctrl+O → Enter → Ctrl+X).

Reload the configuration (or just open a new Terminal tab/window):Bash

```
source ~/.zshrc

```
Verify:Bash

```
echo $ANTHROPIC_BASE_URL
echo $ANTHROPIC_AUTH_TOKEN

```

Now to run model you can do run command specific to the model name.

For example: 

```

claude --model mistral:7b


```

```

claude --model gpt-oss:20b

```



```

claude --model qwen2.5-coder:7b

```
