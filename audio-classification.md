# Fill mask task

## Final API call

```python
import json
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/superb/hubert-large-superb-er"
def query(filename):
    with open(filename, "rb") as f:
        data = f.read()
    response = requests.request("POST", API_URL, headers=headers, data=data)
    return json.loads(response.content.decode("utf-8"))
data = query("sample1.flac")
```

## Original Handlebars template

```handlebars
{{audio src="sample1.flac"}}
```

or...

```handlebars
{{#message role="user"}}
    {{audio src="sample1.flac"}}
{{/message}}
```

## Original f-string template

```html
<audio controls>
    <source src="sample1.flac" type="audio/flac"/>
</audio>
```

or...

```html
<message role="user">
    <audio controls>
        <source src="sample1.flac" type="audio/flac"/>
    </audio>
</message>
```

## Rendered template

```html
<message role="user">
    <audio controls>
        <source src="sample1.flac" type="audio/flac"/>
    </audio>
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "<audio controls>\n    <source src=\"sample1.flac\" type=\"audio/flac\">\n</audio>"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {"score": 0.5928, "label": "neu"},
        {"score": 0.2003, "label": "hap"},
        {"score": 0.128, "label": "ang"},
        {"score": 0.079, "label": "sad"},
    ],
}
```

## Response from gpt-4

N/A