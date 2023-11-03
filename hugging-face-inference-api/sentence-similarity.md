# Fill mask task

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/sentence-transformers/all-MiniLM-L6-v2"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query(
    {
        "inputs": {
            "source_sentence": "That is a happy person",
            "sentences": ["That is a happy dog", "That is a very happy person", "Today is a sunny day"],
        }
    }
)
```

## Original Handlebars template

```handlebars
{{input}}

{{#message role="system"}}
    <select>
        {{#each options}}
            <option>{{this}}</option>
        {{/each}}
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
        {{#each options}}
            <option>{{this}}</option>
        {{/each}}
    </select>
{{/message}}
```

## Original f-string template

```html
{input}

<message role="system">
    <select>
        <option>That is a happy dog</option>
        <option>That is a very happy person</option>
        <option>Today is a sunny day</option>
    </select>
</message>
```

or...

```html
<message role="user">
    {input}
</message>

<message role="system">
    <select>
        <option>That is a happy dog</option>
        <option>That is a very happy person</option>
        <option>Today is a sunny day</option>
    </select>
</message>
```

## Rendered template

```html
<message role="user">
    That is a happy person
</message>

<message role="system">
    <select>
        <option>That is a happy dog</option>
        <option>That is a very happy person</option>
        <option>Today is a sunny day</option>
    </select>
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "How many stars does the transformers repository have?"
        },
        {
            "role": "system",
            "content": "<select>
                <option>That is a happy dog</option>
                <option>That is a very happy person</option>
                <option>Today is a sunny day</option>
            </select>"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        0.6945773363113403,
        0.9429150819778442,
        0.2568760812282562
    ]
}
```

## Response from gpt-4

```
That is a very happy person
```