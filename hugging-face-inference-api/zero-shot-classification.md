# Fill mask task

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/facebook/bart-large-mnli"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query(
    {
        "inputs": "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!",
        "parameters": {"candidate_labels": ["refund", "legal", "faq"]},
    }
)
```

## Original Handlebars template

```handlebars
{{input}}

{{#message role="system"}}
    <select>
        <option>refund</option>
        <option>faq</option>
        <option>legal</option>
    </select>
{{/message}}
```

or...

```handlebars
{{#message role="user"}}
    {{input}}
{{/message}}

{{#message role="system"}}
    <select>
        <option>refund</option>
        <option>faq</option>
        <option>legal</option>
    </select>
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

<message role="system">
    <select>
        <option>refund</option>
        <option>faq</option>
        <option>legal</option>
    </select>
</message>
```

## Rendered template

```html
<message role="user">
    Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!
</message>

<message role="system">
    <select>
        <option>refund</option>
        <option>faq</option>
        <option>legal</option>
    </select>
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!"
        },
        {
            "role": "system",
            "content": "<select>
                <option>refund</option>
                <option>faq</option>
                <option>legal</option>
            </select>"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "sequence": "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!",
            "labels": ["refund", "faq", "legal"],
            "scores": [
                # 88% refund
                0.8778,
                0.1052,
                0.017,
            ],
        },
    ]
}
```

## Response from gpt-4

N/A