# Table Question Answering

## Final API call

```python
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/google/tapas-base-finetuned-wtq"
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query(
    {
        "inputs": {
            "query": "How many stars does the transformers repository have?",
            "table": {
                "Repository": ["Transformers", "Datasets", "Tokenizers"],
                "Stars": ["36542", "4512", "3934"],
                "Contributors": ["651", "77", "34"],
                "Programming language": [
                    "Python",
                    "Python",
                    "Rust, Python and NodeJS",
                ],
            },
        }
    }
)
```

## Original Handlebars template

```handlebars
{{input}}

{{#message role="system"}}
    {{table data={{repoData}}}}
{{/message}}
```

or...

```handlebars
{{#message role="user"}}
    {{input}}
{{/message}}

{{#message role="system"}}
    {{table data={{repoData}}}}
{{/message}}
```

## Original f-string template

```html
{input}

<message role="system">
    <table>
        <tr>
            <th>Repository</th>
            <th>Stars</th>
            <th>Contributors</th>
            <th>Programming language</th>
        </tr>
        <tr>
            <td>Transformers</td>
            <td>36542</td>
            <td>651</td>
            <td>Python</td>
        </tr>
        <tr>
            <td>Datasets</td>
            <td>4512</td>
            <td>77</td>
            <td>Python</td>
        </tr>
        <tr>
            <td>Tokenizers</td>
            <td>3934</td>
            <td>34</td>
            <td>Rust, Python and NodeJS</td>
        </tr>
    </table>
</message>
```

or...

```html
<message role="user">
    {input}
</message>

<message role="system">
    <table>
        <tr>
            <th>Repository</th>
            <th>Stars</th>
            <th>Contributors</th>
            <th>Programming language</th>
        </tr>
        <tr>
            <td>Transformers</td>
            <td>36542</td>
            <td>651</td>
            <td>Python</td>
        </tr>
        <tr>
            <td>Datasets</td>
            <td>4512</td>
            <td>77</td>
            <td>Python</td>
        </tr>
        <tr>
            <td>Tokenizers</td>
            <td>3934</td>
            <td>34</td>
            <td>Rust, Python and NodeJS</td>
        </tr>
    </table>
</message>
```

## Rendered template

```html
<message role="user">
    How many stars does the transformers repository have?
</message>

<message role="system">
    <table>
        <tr>
            <th>Repository</th>
            <th>Stars</th>
            <th>Contributors</th>
            <th>Programming language</th>
        </tr>
        <tr>
            <td>Transformers</td>
            <td>36542</td>
            <td>651</td>
            <td>Python</td>
        </tr>
        <tr>
            <td>Datasets</td>
            <td>4512</td>
            <td>77</td>
            <td>Python</td>
        </tr>
        <tr>
            <td>Tokenizers</td>
            <td>3934</td>
            <td>34</td>
            <td>Rust, Python and NodeJS</td>
        </tr>
    </table>
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
            "content": "<table>
                <tr>
                    <th>Repository</th>
                    <th>Transformers</th>
                    <th>Starts</th>
                    <th>Contributors</th>
                    <th>Programming language</th>
                </tr>
                <tr>
                    <td>Transformers</td>
                    <td>Datasets</td>
                    <td>Tokenizers</td>
                </tr>
                <tr>
                    <td>36542</td>
                    <td>4512</td>
                    <td>3934</td>
                </tr>
                <tr>
                    <td>651</td>
                    <td>77</td>
                    <td>34</td>
                </tr>
                <tr>
                    <td>Python</td>
                    <td>Python</td>
                    <td>Rust, Python and NodeJS</td>
                </tr>
            </table>"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {
            "answer": "AVERAGE > 36542",
            "coordinates": [[0, 1]],
            "cells": ["36542"],
            "aggregator": "AVERAGE",
        },
    ]
}
```

## Response from gpt-4

```
The Transformers repository has 36,542 stars.
```