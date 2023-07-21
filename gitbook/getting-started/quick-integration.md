# 📎 Quick Integration

### Quick Integration

In this section, we'll show you how to perform a quick integration with Portkey using Middleware Mode. We'll take an example of an OpenAI request, and show you how to route it through Portkey using different methods. ([Click here](broken-reference) for integration examples with other providers like Anthropic, Cohere etc.)

* **OpenAI SDK**

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python"><strong>import openai
</strong>
# Set Portkey as the base path
openai.api_base = "https://api.portkey.ai/v1/proxy"

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Translate the following English text to French: '{}'",
  temperature=0.5,
  headers={
    "x-portkey-api-key": "&#x3C;PORTKEY_API_KEY>",
    "x-portkey-mode": "proxy openai"
  }
)

print(response.choices[0].text.strip())
</code></pre>
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const { Configuration, OpenAIApi } = require("openai");
const configuration = new Configuration({
  apiKey: "<OPENAI_API_KEY>",
  basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<PORTKEY_API_KEY>",
        "x-portkey-mode": "proxy openai"
      }
    }
});

const openai = new OpenAIApi(configuration);

async function generateCompletion() {
  const response = await openai.createCompletion({
    model: "text-davinci-003",
    prompt: "Two roads diverged in the yellow woods",
    max_tokens: 512,
    temperature: 0,
  });

  console.log(response.data.choices[0].text);
}

generateCompletion();
```
{% endtab %}
{% endtabs %}

* **Langchain**

{% tabs %}
{% tab title="Python" %}
```python
import os
from langchain.llms import OpenAI
import langchain.utilities import Portkey

os.environ["OPENAI_API_KEY"] = "<OPENAI_API_KEY>"
PORTKEY_API_KEY = "<PORTKEY_API_KEY>" # Set Portkey API key here
TRACE_ID = "portkey_langchain_demo"  # Set trace id here

# Since Portkey is integrated with Langchain, Portkey.Config() takes care of defining headers

headers = Portkey.Config(
    api_key=PORTKEY_API_KEY,
    trace_id=TRACE_ID,
)

# Now, let's pass these headers

llm = OpenAI(headers=headers)

tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(
    tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True
)

# Let's test it out!
agent.run(
    "What was the high temperature in SF yesterday in Fahrenheit? What is that number raised to the .023 power?"
)
```
{% endtab %}

{% tab title="NodeJS" %}
```typescript
import { OpenAI } from "langchain/llms/openai";

const model = new OpenAI({
  modelName: "text-davinci-003", 
  temperature: 0.9,
  openAIApiKey: "<OPENAI_API_KEY>",
  configuration: {
    basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        'x-portkey-api-key': '<PORTKEY_API_KEY>',
        'x-portkey-mode': 'proxy openai',
        'x-portkey-trace-id' : 'langchain_demo'
      }
    }
  }
});

async function main() {
  const res = await model.call("Describe the world as written by Herodotus.");
  console.log(res);
}
main();
```
{% endtab %}
{% endtabs %}

* **Rest API**

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'http://api.portkey.ai/v1/proxy/completions' \
--header 'x-portkey-api-key: <PORTKEY_API_KEY>' \
--header 'x-portkey-mode: proxy openai' \
--header 'Authorization: <OPENAI_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "n": 1,
    "model": "text-davinci-003",
    "prompt": "Top 20 tallest buildings in the world"
}'
```
{% endtab %}
{% endtabs %}


