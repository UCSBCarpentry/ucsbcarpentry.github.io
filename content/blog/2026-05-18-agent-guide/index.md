---
title:  What is an AI Agent?
---

The prolific software developer and writer on AI-assisted coding, Simon Willison
describes [AI agents](https://simonwillison.net/tags/ai-agents/) as "**LLMs
calling tools in a loop to achieve a goal**". It's a good definition,
particularly if you are already familiar with the technical sense of the terms.
If you aren't, you might be wondering: What is a "tool", and how does a language
model "call" one? In this post, I want to add some technical specificity to
Willison's definition by showing how to build a very simple agent: we'll build a
program in which an LLM calls tools in a loop to achieve a goal.

Real-world agents (for example, coding agents) can be quite complicated pieces
of software. However, the core features of an agent are surprisingly simple to
implement. You might be surprised at how little code it takes to build an agent
that can do useful work! Because our goal is pedagogical, not practical, we'll
use plain Python as much as possible. If you're goal is to build a sophisticated
agent with little effort, you should probably use one of the many agent SDKs
designed for the purpose -- or just ask your coding agent to!

To follow this guide, you need an environment to run Python scripts and API
access to a large language model (LLM) provider. We will be using the DREAM
Lab’s AI gateway as our LLM provider, but other model providers should also
work.

## Using LLMs through APIs

Most computer programs, like agents, that *use* LLMs do so through web-based
APIs. Instead of running models directly, the program makes HTTP “requests” to
an LLM model provider over the web. Agents talk to model providers the same way
your web browser talks to web servers: using HTTP. An advantage of this approach
is that it makes the software easier to write and run. We don’t need specialized
hardware for running models, and we don’t need to complex machine learning
frameworks (like PyTorch). Instead, we just need an HTTP client, like Python’s
[requests](https://pypi.org/project/requests/) library.

Model providers (like OpenAI, Anthropic, Google, AWS, etc.) expect programs to
use specific APIs to interact with their LLMs. OpenAI’s [Chat Completion
API](https://developers.openai.com/api/reference/resources/chat), is one of the
oldest and most widely supported APIs for interacting with LLMs--and it’s the API
we’ll use here.

To make HTTP requests to an LLM provider using the Chat Completion API, you need
three things: the API’s base URL, the name of the model you want use, and an API
access key. 

The core of our agent is a Python function, `call_llm()`, that uses the
`requests` library to make HTTP requests to an LLM model provider using the Chat
Completion API. 

```python
import requests
import os

def call_llm(messages, api_base_url, api_model, api_key, tools=None):
    """Makes a request using the Chat Completion API.

    Args:
        messages (list): A list of "message" objects, described in more detail below.
        api_base_url (str): The URL of our API endpoint (ex: `https://litellm.dreamlab.ucsb.edu`).
        api_model (str): Name of the model to use (ex: `gemini-3-flash-preview`).
        api_key (str): An API key to authorize the request.
        tools (list, optional): An optional list of tool definitions.

    Returns:
        dict: The first message object in the response choices.
    """

    # http request url and headers
    request_url = f"{api_base_url}/v1/chat/completions"    
    headers = {
        "Authorization": f"Bearer {api_key}", 
        "Content-Type": "application/json"
    }
    
    # http request body: the data submitted to the API
    data = {
        "model": api_model,
        "messages": messages,
    }

    # include "tools" only if defined
    if tools:
        data["tools"] = tools

    # call the API and print server error if we get one
    response = requests.post(request_url, headers=headers, json=data)
    response.raise_for_status() # raise an error if http response status != 200
   
    # The Chat Completion API supports multiple "choices".
    # We only expect one: return the first message in 'choices'
    resp = response.json()
    return resp["choices"][0]["message"]
```

The `call_llm()` takes several arguments but the primary input for the LLM is
the list of `messages`; the function also returns a new message object with the
output from the LLM. Let's take a closer look at what these "message" objects
consist of.

## Chat Completion Message Structure

The Chat Completion API expects a list of message objects representing the
conversation history. The message list represents the full context of a
multi-turn dialogue, typically between a "user" and the LLM "assistant". The
entire conversation history (the `messages` list) must be included with each
request, otherwise the LLM won't "remember" the full context of the
conversation.

Messages are json objects (Python dicts) with `role` and `content` keys:

- **`role`**: Specifies who is sending the message. This can be one of four main
  roles: `user`, `assistant`, `tool`, or `system`.
- **`content`**: The actual text content of the message.
- Messages may include additional keys for tool calling. We'll come back to this.

We'll talk about the "tool" role a little later (and we're mostly ignoring the
"system" role in this guide). In a simple, chat-based exchange (without tool
calls), the "messages" lists consists of alternating "user" and "assistant"
messages. To illustrate, let's use `call_llm()`, with a single prompt: "What is
the weather in Paris?"

```python
import os

# messages with initial prompt (user role)
prompt = "What is the weather in Paris"
messages = [{"role": "user", "content": prompt}]

# api config
api_base_url = "https://litellm.dreamlab.ucsb.edu"
api_model = "gemini-3-flash-preview"
api_key = os.getenv("LLM_API_KEY")  # key stored as environment variable

msg = call_llm(messages, api_base_url, api_model, api_key)

# msg has assistant role with API response
print(msg) # {"role": "assistant", "content": "The weather in Paris is ..."}
```

To continue the conversation, we would append the assistant response (`msg`) to
the `messages` list and then add an additional user message:

```python
# messages = [{"role": "user", "content": "What is the weather in Paris?"}]
# msg = call_llm(messages, api_base_url, api_model, api_key)

# append assistant response message list
messages.append(msg) 

# append new user prompt to message list
messages.append({"role": "user", "content": "temperature in C and F please!"})

# second assistant response
msg = call_llm(messages, api_base_url, api_model, api_key)
messages.append(msg)
```

The final `messages` list would include the following:

| `role`      | `content`                                 |
| :---------- | :---------------------------------------- |
| `user`      | `"What is the weather in Paris?"`         |
| `assistant` | `"The weather in Paris is ..."`           |
| `user`      | `"temperature in C and F please!"`        |
| `assistant` | `"It is 13°C (55°F) with clear skies..."` |


## Use 'Tools' to Avoid Hallucinations

When I ran this script above, with the prompt "What is the weather in Paris?", I received the response:

```md
As of right now in Paris, France:

*   **Temperature:** 13°C (55°F)
*   **Conditions:** Clear skies and sunny.
*   **Wind:** 11 km/h (7 mph)
*   **Humidity:** 61%

**Forecast for the rest of today:**
It is expected to stay clear and cool throughout the evening, with temperatures dropping to a low of about 7°C (45°F) overnight. 

**Tomorrow's Outlook:**
Similar weather is expected tomorrow, with mostly sunny skies and a high of 14°C (57°F).
```

At the time, this descriptions was not accurate. In fact, running the script
multiple times returned completely different weather conditions! That's because
the model doesn't actually know what the weather in Paris is, so it makes up the
answer. It "hallucinates" a plausible description of the weather. One way to
avoid hallucinations in LLM API responses is to provide the model with "tools"
that it can use. Tools provide LLMs with ways to access current information,
perform tasks, and avoid having to fill-in missing details with statistically
likely text. 

To illustrate how tools work, we'll create a tool called `get_weather` that
returns current weather conditions for a given location. For now, we're not
concerned with *implementing* the tool. First, we just want to change our
request so that the LLM API is aware of the tool.

The optional `tools` argument of our `call_llm()` function is used to provide
the LLM API with structured descriptions of tools it can call. In this context,
you can think of "tools" as metadata describing a function in terms of inputs
and outputs. Here's how we would describe our `get_weather` tool using the Chat
Completion API:

```python
# get_weather_schema describes the `get_weather` tool.
get_weather_schema = {
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "A place name (e.g., Paris)",
                }
            },
            "required": ["location"],
        },
    },
}
```

Now let's see how our response changes when we include this tool (`get_weather_schema`).

```python
prompt = "What is the weather in Paris"
messages = [{"role": "user", "content": prompt}]

# same prompt, api_base_url, api_model, and api_key as before
msg = call_llm(messages, api_base_url, api_model, api_key, tools = [get_weather_schema])

# print response details
print(msg["role"])    # "assistant"
print(msg["content"]) # None
print(msg["too_calls"][0][function]) # {'arguments': '{"location": "Paris"}', 'name': 'get_weather'}
```

The response has changed in a few ways. First, doesn't include any `content`
(the `content` key is still present in the response message, but its values is
`None`). Second, there is a new key, `tool_calls`, which is a list of objects
like this:

```json
{"arguments": "{'location': 'Paris'}", "name": "get_weather"}`
```

As the name suggests, "tool calls" are how the API calls (or invokes) the tools
we included in the request. Our request included the `get_weather` tool
definition, and the response includes a tool call to run the `get_weather`
function with arguments `{"location": "Paris"}`. The expectation is that we will
run `get_weather()` and make an additional request with the output
from the tool call. 

It's time to implement the `get_weather()` function so that we can call it from
our python code. We'll use https://wttr.in as it provides a free, simple API
that is sufficient for our purposes:

```py
# get_weather is our python implemention of the `get_weather` tool.
# It gets the current weather for a given location using a weather
# API (wttr.in)
def get_weather(location: str) -> str:
    url = f"https://wttr.in/{location}?format=3"
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        return response.text.strip()
    except Exception as e:
        return f"Could not get weather for {location}: {e}"

# example
get_weather("Paris") # "paris: ☁️  +56°F"
```

Now that we have implemented `get_weather`, we can call the Python function
using the arguments in the tool call, and then send the output back to the LLM.
The tool call output is included in the message of a second request, using the
`"tool"` role:


```python
# continuing from above ...
# msg = call_llm(messages, api_base_url, api_model, api_key, tools = [get_weather_schema])

# run tool call in response:
for call in msg["tool_calls"]:

    # parse tool call function name and arguments
    name = call["function"]["name"]
    if name != "get_weather":
        raise ValueError(f"unexpected function name: {name}")
    args = json.loads(call["function"]["arguments"])
    result = get_weather(**args)
    
    # message with tool cal output
    new_msg =  {
        "role": "tool",
        "tool_call_id": call["id"],
        "content": str(result),
    }
    messages.append(new_msg)

# final request with tool call results
msg = call_llm(messages, api_base_url, api_model, api_key, tools = [get_weather_schema])
messages.append(msg)
print(msg["content"]) # The weather in Paris is currently 56°F and cloudy.
```

The final message sequence to/from the LLM API is represented in the table
below. Note that the tool output is sent to the LLM API using a message with
`"tool"` role, not the typical `"user"` role.

| `role`                             | `content`                                           | `tool_calls`                                                    |
| :--------------------------------- | :-------------------------------------------------- | :-------------------------------------------------------------- |
| `user`                             | "What is the weather in Paris?"                     |                                                                 |
| `assistant`                        | *None*                                              | `{"arguments": '{"location": "Paris"}', "name": 'get_weather'}` |
| `tool`                             | "Paris: ☁️  +56°F"                                |                                                                 |
| `assistant`                        | "The weather in Paris is currently 56°F and cloudy" |                                                                 |

## Calling Tools in a Loop 

Let's revisit Willison's definition of AI agents: they are "**LLMs calling tools
in a loop to achieve a goal**. At this point, we have a better understanding of
how LLMs call tools: we include descriptions of available tools in our LLM API
requests; the response messages may include `tool_calls`, which we process
locally; we then send tool call output back to the LLM API as messages with the
`"tool"` role. In the code above, we only processed a single response message
from the LLM API. In fact, that's not how AI agents tend to work. They call
tools "in a loop" -- sometimes called the "agent loop".

... 
