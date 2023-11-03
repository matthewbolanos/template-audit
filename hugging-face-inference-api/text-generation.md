# Text generation

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/gpt2"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query({"inputs": "The answer to the universe is"})
```

## Original Handlebars template

```handlebars
{{input}}
```

or...

```handlebars
{{#message role="user"}}
    {{input}}
{{/message}}
```

## Original f-string template

```html
{input}
```

or...

```html
<message role="user">
    {input}
</message>
```

## Rendered template

```html
<message role="user">
    The answer to the universe is
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "The answer to the universe is"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "generated_text": "The answer to the universe is that we are the creation of the entire universe," says Fitch.\n\nAs of the 1960s, six times as many Americans still make fewer than six bucks ($17) per year on their way to retirement."
        }
    ]
}
```

## Response from gpt-4

```
42, according to "The Hitchhiker's Guide to the Galaxy" by Douglas Adams.
```