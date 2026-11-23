# Claude API call

This page explains one API call, the building block behind the FreshFlow writer, checker and translator steps.

## Example

```python
import anthropic
from google.colab import userdata

# Read the API key from a secret. Never type the key into the code.
client = anthropic.Anthropic(api_key=userdata.get("ANTHROPIC_API_KEY"))

response = client.messages.create(
    model="claude-haiku-5-5",
    max_tokens=500,
    system="You are the FreshFlow content writer. Follow the brand guide and the 10 compliance rules.",
    messages=[
        {"role": "user", "content": "Write a 2-line product intro for our inventory tracking software."}
    ],
)

print(response.content[0].text)
```

If you run this outside Google Colab, replace the `userdata` line with an environment variable:

```python
import os
client = anthropic.Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])
```

## What each part does

| Part | Meaning |
|---|---|
| `anthropic.Anthropic(...)` | Creates a connection to the API using your key |
| `model` | Which Claude model answers. FreshFlow uses `claude-haiku-5-5` |
| `max_tokens` | The longest reply allowed. A token is about three-quarters of a word |
| `system` | Standing instructions: role, brand guide and rules |
| `messages` | The conversation. Here it holds one user request |
| `response.content[0].text` | The text of Claude's reply |

## How FreshFlow uses it

| Step | `system` holds | `messages` holds |
|---|---|---|
| Writer | Brand guide, rules, and the output format (JSON) | The content request |
| Checker | The 10 rules and the pass, fail or unsure format | The writer's draft |
| Translator | The glossary of terms that must stay exact | The approved draft |

## Common errors

| Error | Likely cause | Fix |
|---|---|---|
| Authentication error | Wrong or missing key | Check the secret name and value |
| Credit error | No API credit on the account | Add credit in the Anthropic Console |
| Model not found | Wrong model name | Check the spelling of `model` |
| Reply cut off | `max_tokens` too low | Increase `max_tokens` |

## Safety

> [!WARNING]
> Never put your API key in code or commit it to GitHub. Keep it in a secret or an environment variable. If the key is ever shown publicly, delete it in the Console and create a new one.
