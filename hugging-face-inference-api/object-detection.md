# Fill mask task

## Final API call

```python
import json
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/facebook/detr-resnet-50"
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
        {
            "score": 0.9982,
            "label": "remote",
            "box": {"xmin": 40, "ymin": 70, "xmax": 175, "ymax": 117},
        },
        {
            "score": 0.9960,
            "label": "remote",
            "box": {"xmin": 333, "ymin": 72, "xmax": 368, "ymax": 187},
        },
        {
            "score": 0.9955,
            "label": "couch",
            "box": {"xmin": 0, "ymin": 1, "xmax": 639, "ymax": 473},
        },
        {
            "score": 0.9988,
            "label": "cat",
            "box": {"xmin": 13, "ymin": 52, "xmax": 314, "ymax": 470},
        },
        {
            "score": 0.9987,
            "label": "cat",
            "box": {"xmin": 345, "ymin": 23, "xmax": 640, "ymax": 368},
        },
    ],
}
```

## Response from gpt-4

N/A