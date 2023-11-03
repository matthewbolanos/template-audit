# Text generation with functions

## Final API call

```python
from transformers import AutoModelForCausalLM , AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("glaiveai/glaive-function-calling-v1", trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained("glaiveai/glaive-function-calling-v1", trust_remote_code=True).half().cuda()

inputs = tokenizer(prompt,return_tensors="pt").to(model.device)

outputs = model.generate(**inputs,do_sample=True,temperature=0.1,top_p=0.95,max_new_tokens=100)

print(tokenizer.decode(outputs[0],skip_special_tokens=True))
```

Where the prompt is the following

```txt
SYSTEM: You are an helpful assistant who has access to the following functions to help the user, you can use the functions if needed-
{
            "name": "expedia-plan_holiday",
            "description": "Plan a holiday based on user's interests",
            "parameters": {
                "type": "object",
                "properties": {
                    "destination": {
                        "type": "string",
                        "description": "The destination of the holiday",
                    },
                    "duration": {
                        "type": "integer",
                        "description": "The duration of the trip in holiday",
                    },
                },
                "required": ["destination", "duration"],
            },
}
USER: I am thinking of having a 10 day long vacation in Greece, can you help me plan it?
ASSISTANT: <functioncall> {"name": "expedia-plan_holiday", "arguments": '{
  "destination": "Greece",
  "duration": 10
}'}
FUNCTION CALL: {"places_to_visit":["Athens","Santorini","Mykonos"]}
```

## Original Handlebars template

```handlebars
{{#message role="user"}}
    {{input}}
{{/message}}

{{#message role="assistant"}}
    {{function "expedia" "plan_holiday" arguments=arguments}}}
{{/message}}

{{#message role="function" pluginName="expedia" name="plan_holiday"}}
    {{results}}
{{/message}}


{{#functions}}
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
    <function pluginName="expedia" name="plan_holiday">
        {arguments}
    </function>
</message>

<message role="function" pluginName="expedia" name="plan_holiday">
    {results}
</message>

<functions>
    <function pluginName="expedia" name="plan_holiday" />
</functions>
```

## Rendered template

```html

<message role="user">
    I am thinking of having a 10 day long vacation in Greece, can you help me plan it?
</message>

<message role="assistant">
    <function pluginName="expedia" name="plan_holiday">
        {
            "destination": "Greece",
            "duration": 10
        }
    </function>
</message>

<message role="function" pluginName="expedia" name="plan_holiday">
    {"places_to_visit":["Athens","Santorini","Mykonos"]}
</message>

<functions>
    <function pluginName="expedia" name="plan_holiday" />
</functions>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "user",
            "content": "Plan a holiday based on user's interests."
        },
        {
            "role": "assistant",
            "function_call": {
                "name": "expedia-plan_holiday",
                "arguments": {
                    "destination": "Greece",
                    "duration": 10
                }
            }
        },
        {
            "role": "function",
            "name": "expedia-plan_holiday",
            "content": {
                "places_to_visit": [
                    "Athens",
                    "Santorini",
                    "Mykonos"
                ]
            }
        }
    ],
    "functions": [
    {
        "name": "expedia-plan_holiday",
        "description": "Describes places to visit on a holiday",
        "parameters": {
            "type": "object",
            "properties": {
                "destination": {
                    "type": "string",
                    "description": "The destination of the holiday",
                },
                "duration": {
                    "type": "integer",
                    "description": "The duration of the trip in holiday",
                }
            }
        },
    }
]
}
```

## Response from gpt-4

_not run_