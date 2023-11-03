# Fill mask task

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/distilbert-base-uncased-finetuned-sst-2-english"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query({"inputs": "I like you. I love you"})
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
    I like you. I love you
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "I like you. I love you"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {"label": "POSITIVE", "score": 0.9999},
        {"label": "NEGATIVE", "score": 0.0001},
    ]
}
```

## Response from gpt-4

N/A