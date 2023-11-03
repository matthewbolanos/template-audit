# Chat completion with functions

## Final API call

```python
messages = [
    {
        "role": "user",
        "content": "Find beachfront hotels in San Diego for less than $300 a month with free breakfast."
    },
    {
        "role": "assistant",
        "function_call": {
            "name": "expedia-search_hotels",
            "arguments": "{\n  \"location\": \"San Diego\",\n  \"max_price\": 300,\n  \"features\": \"beachfront,free breakfast\"\n}"
        }
    },
    {
        "role": "function",
        "name": "expedia-search_hotels",
        "content": "<results of function call>"
    }
]
functions = [
    {
        "name": "expedia-search_hotels",
        "description": "Retrieves hotels from the search index based on the parameters provided",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The location of the hotel (i.e. Seattle, WA)"
                },
                "max_price": {
                    "type": "number",
                    "description": "The maximum price for the hotel"
                },
                "features": {
                    "type": "string",
                    "description": "A comma separated list of features (i.e. beachfront, free wifi, etc.)"
                }
            },
            "required": ["location"]
        }
    }
]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo-0613",
    messages=messages,
    functions=functions,
    function_call="auto",  # auto is default, but we'll be explicit
)
response_message = response["choices"][0]["message"]
```

## Original Handlebars template

```handlebars
{{#message role="user"}}
    {{input}}
{{/message}}

{{#message role="assistant"}}
    {{function "expedia" "search_hotels" arguments=arguments}}}
{{/message}}

{{#message role="function" pluginName="expedia" name="search_hotels"}}
    {{results}}
{{/message}}

{{#functions functionCall="auto"}}
    {{#each functions}}
        {{function pluginName=pluginName name=name}}
    {{/each}}
{{/functions}}
```

## Original f-string template

```html
<message role="user">
    {input}
</message>

<message role="assistant">
    <function pluginName="expedia" name="search_hotels">
        {arguments}
    </function>
</message>

<message role="function" pluginName="expedia" name="search_hotels">
    {results}
</message>

<functions functionCall="auto">
    <function pluginName="expedia" name="search_hotels" />
</functions>
```

## Rendered template

```html

<message role="user">
    Find beachfront hotels in San Diego for less than $300 a month with free breakfast.
</message>

<message role="assistant">
    <function pluginName="expedia" name="search_hotels">
        {
            "location": "San Diego",
            "max_price": 300,
            "features": "beachfront,free breakfast"
        }
    </function>
</message>

<message role="function" pluginName="expedia" name="search_hotels">
    ...results of function call...
</message>

<functions functionCall="auto">
    <function pluginName="expedia" name="search_hotels" />
</functions>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "Find beachfront hotels in San Diego for less than $300 a month with free breakfast."
        },
        {
            "role": "assistant",
            "function_call": {
                "name": "expedia-search_hotels",
                "arguments": {
                    "location": "San Diego",
                    "max_price": 300,
                    "features": "beachfront,free breakfast"
                }
            }
        },
        {
            "role": "function",
            "name": "expedia-search_hotels",
            "content": "<results of function call>"
        }
    ],
    "functions": [
    {
        "name": "expedia-search_hotels",
        "description": "Retrieves hotels from the search index based on the parameters provided",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The location of the hotel (i.e. Seattle, WA)"
                },
                "max_price": {
                    "type": "number",
                    "description": "The maximum price for the hotel"
                },
                "features": {
                    "type": "string",
                    "description": "A comma separated list of features (i.e. beachfront, free wifi, etc.)"
                }
            },
            "required": ["location"]
        }
    }
]
}
```

## Response from gpt-4

_not run_