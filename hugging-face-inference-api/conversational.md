# Conversational API

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/microsoft/DialoGPT-large"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query(
    {
        "inputs": {
            "past_user_inputs": ["Which movie is the best ?"],
            "generated_responses": ["It's Die Hard for sure."],
            "text": "Can you explain why ?",
        },
    }
)
```

## Original Handlebars template

```handlebars
{{#message role="user"}}
    Which movie is the best?
{{/message}}

{{#message role="assistant"}}
    It's Die Hard for sure.
{{/message}}

{{#message role="user"}}
    {{input}}
{{/message}}
```


## Original f-string template

```html
<message role="user">
    Which movie is the best?
</message>

<message role="assistant">
    It's Die Hard for sure.
</message>

<message role="user">
    {user}
</message>
```

## Rendered template

```html
<message role="user">
    Which movie is the best?
</message>

<message role="assistant">
    It's Die Hard for sure.
</message>

<message role="user">
    Can you explain why ?
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "Which movie is the best?"
        },
        {
            "role": "assistant",
            "content": "It's Die Hard for sure."
        },
        {
            "role": "user",
            "content": "an you explain why ?"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "generated_text": "It's the best movie ever.",
            "conversation": {
                "past_user_inputs": [
                    "Which movie is the best ?",
                    "Can you explain why ?",
                ],
                "generated_responses": [
                    "It's Die Hard for sure.",
                    "It's the best movie ever.",
                ],
            },
            # "warnings": ["Setting `pad_token_id` to `eos_token_id`:50256 for open-end generation."],
        }
    ]
}
```

## Response from gpt-4

```
As an AI, I don't have personal opinions. However, many people enjoy "Die Hard" because it's considered a classic action film. It has a compelling plot, a charismatic performance by Bruce Willis, memorable lines, and intense action sequences. It's also known for its Christmas setting, which has sparked a long-running debate about whether it qualifies as a Christmas movie.
```