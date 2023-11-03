# Fill mask task

## Final API call

```python
import json
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/google/vit-base-patch16-224"
def query(filename):
    with open(filename, "rb") as f:
        data = f.read()
    response = requests.request("POST", API_URL, headers=headers, data=data)
    return json.loads(response.content.decode("utf-8"))
data = query("cats.jpg")
```

## Original Handlebars template

```handlebars
{{img src="cats.jpg"}}
```

or...

```handlebars
{{#message role="user"}}
    {{img src="cats.jpg"}}
{{/message}}
```

## Original f-string template

```html
<img src="cats.jpg" />
```

or...

```html
<message role="user">
    <img src="cats.jpg" />
</message>
```

## Rendered template

```html
<message role="user">
    <img src="cats.jpg" />
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "<img src=\"cats.jpg\">"
        }
    ]
}
```

## Response from Hugging face model

```json
{
    "options": [
        {"score": 0.9374, "label": "Egyptian cat"},
        {"score": 0.0384, "label": "tabby, tabby cat"},
        {"score": 0.0144, "label": "tiger cat"},
        {"score": 0.0033, "label": "lynx, catamount"},
        {"score": 0.0007, "label": "Siamese cat, Siamese"},
    ],
}
```

## Response from gpt-4

N/A