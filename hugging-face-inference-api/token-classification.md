# Fill mask task

## Final API call

```python
Copied
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/dbmdz/bert-large-cased-finetuned-conll03-english"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query({"inputs": "My name is Sarah Jessica Parker but you can call me Jessica"})
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
    My name is Sarah Jessica Parker but you can call me Jessica
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "My name is Sarah Jessica Parker but you can call me Jessica"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "entity_group": "PER",
            "score": 0.9991,
            "word": "Sarah Jessica Parker",
            "start": 11,
            "end": 31,
        },
        {
            "entity_group": "PER",
            "score": 0.998,
            "word": "Jessica",
            "start": 52,
            "end": 59,
        },
    ],
}
```

## Response from gpt-4

N/A