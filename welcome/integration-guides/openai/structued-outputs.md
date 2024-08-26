# Structued Outputs

Structued Outputs ensure that the model always follows your supplied [JSON schema](https://json-schema.org/overview/what-is-jsonschema). Portkey supports OpenAI's Structured Outputs featue out of the box with our SDKs & APIs.&#x20;

{% hint style="info" %}
Structued Outputs is different from OpenAI's `JSON Mode` as well as `Function Calling`. [Check out this table](structued-outputs.md#difference-between-structued-outputs-json-mode-and-function-calling) for a quick comparison.
{% endhint %}



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

### Difference Between Structued Outputs, JSON Mode, and Function Calling

* If you are connecting the model to tools, functions, data, etc. in your system, then you should use **function calling.**
* And if you want to structure the model's output when it responds to the user, then you should use a structured `response_format`.
  * In `response_format`, you can set it as `{ "type": "json_object" }` to enable the [JSON Mode](https://platform.openai.com/docs/guides/structured-outputs/json-mode).
  * And you can set it as `{ "type": "json_schema" }` to use the [Structued Outputs Mode](https://platform.openai.com/docs/guides/structured-outputs) described above.

For more, refer to OpenAI's [detailed documentation on Structued Outputs here](https://platform.openai.com/docs/guides/structured-outputs/supported-schemas).&#x20;
