# Fill mask task

## Final API call

```python
import json
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://api-inference.huggingface.co/models/facebook/wav2vec2-base-960h"
def query(filename):
    with open(filename, "rb") as f:
        data = f.read()
    response = requests.request("POST", API_URL, headers=headers, data=data)
    return json.loads(response.content.decode("utf-8"))
data = query("sample1.flac")
```

## Original Handlebars template

```handlebars
{{#message role="user"}}
    {{audio src="sample1.flac"}}
{{/message}}
```

## Original f-string template

```html
{input}
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
        {
            "text": "GOING ALONG SLUSHY COUNTRY ROADS AND SPEAKING TO DAMP AUDIENCES IN DRAUGHTY SCHOOL ROOMS DAY AFTER DAY FOR A FORTNIGHT HE'LL HAVE TO PUT IN AN APPEARANCE AT SOME PLACE OF WORSHIP ON SUNDAY MORNING AND HE CAN COME TO US IMMEDIATELY AFTERWARDS"
        },
    ]
}
```

## Response from gpt-4

N/A