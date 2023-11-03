# Question Answering

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/deepset/roberta-base-squad2"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query(
    {
        "inputs": {
            "question": "Where is the house located?",
            "context": "The house is located at 116 Mountain View Dr..",
        }
    }
)
```

## Original Handlebars template

```handlebars
{{input}}

{{#message role="system"}}
    The house is located at 116 Mountain View Dr..
{{/message}}
```

or...

```handlebars
{{#message role="user"}}
    {{input}}
{{/message}}

{{#message role="system"}}
    The house is located at 116 Mountain View Dr..
{{/message}}
```

## Original f-string template

```html
{input}

<message role="system">
    The house is located at 116 Mountain View Dr..
</message>
```

or...

```html
<message role="user">
    {input}
</message>

<message role="system">
    The house is located at 116 Mountain View Dr..
</message>
```

## Rendered template

```html
<message role="user">
    Where is the house located?
</message>

<message role="system">
    The house is located at 116 Mountain View Dr..
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "Where is the house located?"
        },
        {
            "role": "system",
            "content": "The house is located at 116 Mountain View Dr.."
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {"score": 0.9327, "start": 11, "end": 16, "answer": "Clara"}
    ]
}
```

## Response from gpt-4

```
As an AI, I don't have access to real-time data or personal data unless it has been shared with me in the course of our conversation.
```