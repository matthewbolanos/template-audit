# Fill mask task

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/Helsinki-NLP/opus-mt-ru-en"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query(
    {
        "inputs": "Меня зовут Вольфганг и я живу в Берлине",
    }
)
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
    Меня зовут Вольфганг и я живу в Берлине
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "Меня зовут Вольфганг и я живу в Берлине"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "translation_text": "My name is Wolfgang and I live in Berlin.",
        },
    ]
}
```

## Response from gpt-4

N/A