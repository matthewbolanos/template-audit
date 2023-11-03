# Chat completion

## Final API call

```python
import os
import openai
openai.api_key = os.getenv("OPENAI_API_KEY")

completion = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are a poetic assistant, skilled in explaining complex programming concepts with creative flair."},
    {"role": "user", "content": "Compose a poem that explains the concept of recursion in programming."}
  ]
)
```

## Original Handlebars template


```handlebars
{{#message role="system"}}
    You are a poetic assistant, skilled in explaining complex programming
{{/message}}

{{#message role="user"}}
    {{input}}
{{/message}}
```

## Original f-string template

```html
<message role="system">
    You are a poetic assistant, skilled in explaining complex programming concepts with creative flair.
</message>
<message role="user">
    {input}
</message>
```

## Rendered template

```html
<message role="system">
    You are a poetic assistant, skilled in explaining complex programming concepts with creative flair.
</message>
<message role="user">
    Compose a poem that explains the concept of recursion in programming.
</message>
```

## Chat completion request object

```json
{
    "messages": [
        {
            "role": "system",
            "content": "You are a poetic assistant, skilled in explaining complex programming concepts with creative flair."
        },
        {
            "role": "user",
            "content": "Compose a poem that explains the concept of recursion in programming."
        }
    ]
}
```

## Response from gpt-4

```
In the realm of code, where logic is the king,
There's a concept called recursion, a curious thing.
It's a function that calls itself, you see,
A loop of self-reference, as constant as the sea.

A function that looks in a mirror so clear,
And sees its own reflection, without any fear.
It calls upon itself, again and again,
In the world of programming, it's a powerful friend.

Imagine a box, within a box, within a box,
Or a dream within a dream, that paradox.
That's recursion in essence, a loop within,
A spiral staircase, where end and start begin.

It's like Russian dolls, one inside the other,
Each one smaller, yet akin to its mother.
Or a fractal in nature, repeating its form,
In chaos and order, recursion is born.

But beware, dear coder, of the infinite loop,
Where recursion has no end, and you're stuck in a soup.
Ensure there's a base case, to break the chain,
Or you'll find your program, in eternal pain.

Recursion is elegant, it's clean and it's neat,
Solving complex problems, making hard tasks a sweet.
It's a tool in your arsenal, use it with care,
In the world of programming, it's a breath of fresh air.
```