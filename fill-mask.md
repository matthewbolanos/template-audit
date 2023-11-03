# Fill mask task

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/bert-base-uncased"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query({"inputs": "The answer to the universe is [MASK]."})
```

## Original Handlebars template

```handlebars
The answer to the {{input}} is [MASK].
```
or...

```
{{#message role="user"}}
    The answer to the {{input}} is [MASK].
{{/message}}
```

## Original f-string template

```html
The answer to the {input} is [MASK].
```

or...

```html
<message role="user">
    The answer to the {input} is [MASK].
</message>
```

## Rendered template

```html
The answer to the universe is [MASK].
```

or...

```html
<message role="user">
    The answer to the universe is [MASK].
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "content"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "sequence": "the answer to the universe is no.",
            "score": 0.1696,
            "token": 2053,
            "token_str": "no"
        },
        {
            "sequence": "the answer to the universe is nothing.",
            "score": 0.0734,
            "token": 2498,
            "token_str": "nothing"
        },
        {
            "sequence": "the answer to the universe is yes.",
            "score": 0.0580,
            "token": 2748,
            "token_str": "yes"
        },
        {
            "sequence": "the answer to the universe is unknown.",
            "score": 0.044,
            "token": 4242,
            "token_str": "unknown"
        },
        {
            "sequence": "the answer to the universe is simple.",
            "score": 0.0402,
            "token": 3722,
            "token_str": "simple"
        }
    ]
}
```

## Response from gpt-4

```
42
```