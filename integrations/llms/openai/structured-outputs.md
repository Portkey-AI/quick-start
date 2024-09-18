# Structured Outputs

Structured Outputs ensure that the model always follows your supplied [JSON schema](https://json-schema.org/overview/what-is-jsonschema). Portkey supports OpenAI's Structured Outputs feature out of the box with our SDKs & APIs.&#x20;

{% hint style="info" %}
Structured Outputs is different from OpenAI's `JSON Mode` as well as `Function Calling`. [Check out this table](structured-outputs.md#difference-between-structued-outputs-json-mode-and-function-calling) for a quick comparison.
{% endhint %}

Portkey SDKs for [Python](https://github.com/openai/openai-python/blob/main/helpers.md#structured-outputs-parsing-helpers) and [JavaScript](https://github.com/openai/openai-node/blob/master/helpers.md#structured-outputs-parsing-helpers) also make it easy to define object schemas using [Pydantic](https://docs.pydantic.dev/latest/) and [Zod](https://zod.dev/) respectively. Below, you can see how to extract information from unstructured text that conforms to a schema defined in code.

{% tabs %}
{% tab title="Pydantic with Python" %}
```python
from portkey_ai import Portkey
from pydantic import BaseModel

class Step(BaseModel):
    explanation: str
    output: str

class MathReasoning(BaseModel):
    steps: list[Step]
    final_answer: str

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="OPENAI_VIRTUAL_KEY"
)

completion = portkey.beta.chat.completions.parse(
    model="gpt-4o-2024-08-06",
    messages=[
        {"role": "system", "content": "You are a helpful math tutor. Guide the user through the solution step by step."},
        {"role": "user", "content": "how can I solve 8x + 7 = -23"}
    ],
    response_format=MathReasoning,
)

print(completion.choices[0].message)
print(completion.choices[0].message.parsed)
```
{% endtab %}

{% tab title="Zod with NodeJS" %}
```typescript
import { Portkey } from 'portkey-ai';
import { z } from 'zod';
import { zodResponseFormat } from "openai/helpers/zod";

const MathReasoning = z.object({
  steps: z.array(z.object({ explanation: z.string(), output: z.string() })),
  final_answer: z.string()
});

const portkey = new Portkey({
    apiKey: "YOUR_API_KEY",
    virtualKey: "YOUR_VIRTUAL_KEY"
});

async function runMathTutor() {
  try {
    const completion = await portkey.chat.completions.create({
      model: "gpt-4o-2024-08-06",
      messages: [
        { role: "system", content: "You are a helpful math tutor." },
        { role: "user", content: "Solve 8x + 7 = -23" }
      ],
      response_format: zodResponseFormat(MathReasoning, "MathReasoning")
    });

    console.log(completion.choices[0].message.content);
  } catch (error) {
    console.error("Error:", error);
  }
}

runMathTutor();
```
{% endtab %}
{% endtabs %}

The second approach, shown in the subsequent examples, uses a JSON schema directly in the API call. This method is more portable across different languages and doesn't require additional libraries, but lacks the integrated type checking of the Pydantic/Zod approach. Choose the method that best fits your project's needs and language ecosystem.

{% tabs %}
{% tab title="NodeJS" %}
```typescript
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: "PORTKEY_API_KEY",
  virtualKey: "OPENAI_VIRTUAL_KEY",
});

async function main() {
  const completion = await portkey.chat.completions.create({
    model: "gpt-4o-2024-08-06",
    messages: [
      { role: "system", content: "Extract the event information." },
      {
        role: "user",
        content: "Alice and Bob are going to a science fair on Friday.",
      },
    ],
    response_format: {
      type: "json_schema",
      json_schema: {
        name: "math_reasoning",
        schema: {
          type: "object",
          properties: {
            steps: {
              type: "array",
              items: {
                type: "object",
                properties: {
                  explanation: { type: "string" },
                  output: { type: "string" },
                },
                required: ["explanation", "output"],
                additionalProperties: false,
              },
            },
            final_answer: { type: "string" },
          },
          required: ["steps", "final_answer"],
          additionalProperties: false,
        },
        strict: true,
      },
    },
  });
  const event = completion.choices[0].message?.content;
  console.log(event);
}

main();

```
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
<strong>    api_key="PORTKEY_API_KEY",
</strong><strong>    virtual_key="OPENAI_VIRTUAL_KEY"
</strong>)

completion = portkey.chat.completions.create(
    model="gpt-4o-2024-08-06",
    messages=[
        {"role": "system", "content": "Extract the event information."},
        {"role": "user", "content": "A meteor the size of 1000 football stadiums will hit earth this Sunday"},
    ],
    response_format={
          "type": "json_schema",
          "json_schema": {
            "name": "math_reasoning",
            "schema": {
              "type": "object",
              "properties": {
                "steps": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "explanation": { "type": "string" },
                      "output": { "type": "string" }
                    },
                    "required": ["explanation", "output"],
                    "additionalProperties": False
                  }
                },
                "final_answer": { "type": "string" }
              },
              "required": ["steps", "final_answer"],
              "additionalProperties": False
            },
            "strict": True
          }
        },
)

print(completion.choices[0].message.content)

</code></pre>
{% endtab %}

{% tab title="REST API" %}
```sh
curl https://api.portkey.ai/v1/chat/completions \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: $OPENAI_VIRTUAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-2024-08-06",
    "messages": [
      {
        "role": "system",
        "content": "You are a helpful math tutor. Guide the user through the solution step by step."
      },
      {
        "role": "user",
        "content": "how can I solve 8x + 7 = -23"
      }
    ],
    "response_format": {
      "type": "json_schema",
      "json_schema": {
        "name": "math_reasoning",
        "schema": {
          "type": "object",
          "properties": {
            "steps": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "explanation": { "type": "string" },
                  "output": { "type": "string" }
                },
                "required": ["explanation", "output"],
                "additionalProperties": false
              }
            },
            "final_answer": { "type": "string" }
          },
          "required": ["steps", "final_answer"],
          "additionalProperties": false
        },
        "strict": true
      }
    }
  }'
```
{% endtab %}
{% endtabs %}

### Difference Between Structured Outputs, JSON Mode, and Function Calling

* If you are connecting the model to tools, functions, data, etc. in your system, then you should use **function calling.**
* And if you want to structure the model's output when it responds to the user, then you should use a structured `response_format`.
  * In `response_format`, you can set it as `{ "type": "json_object" }` to enable the [JSON Mode](https://platform.openai.com/docs/guides/structured-outputs/json-mode).
  * And you can set it as `{ "type": "json_schema" }` to use the [Structured Outputs Mode described above](https://platform.openai.com/docs/guides/structured-outputs).

For more, refer to OpenAI's [detailed documentation on Structured Outputs here](https://platform.openai.com/docs/guides/structured-outputs/supported-schemas).&#x20;
