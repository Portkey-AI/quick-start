---
description: >-
  Learn to integrate OpenAI with Portkey, enabling seamless completions, prompt
  management, and advanced functionalities like streaming, function calling and
  fine-tuning.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# OpenAI

Portkey has native integrations with OpenAI SDKs for Node.js, Python, and its REST APIs. For OpenAI integration using other frameworks, explore our partnerships, including [Langchain](../../libraries/langchain-python.md), [LlamaIndex](../../libraries/llama-index-python.md), among [others](../).

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`openai`**</mark>
{% endhint %}

## Using the Portkey Gateway

To integrate the Portkey gateway with OpenAI,&#x20;

* Set the `baseURL` to the Portkey Gateway URL
* Include Portkey-specific headers such as `provider`, `apiKey`and others.

Here's how to apply it to a **chat completion** request:

{% tabs %}
{% tab title="OpenAI NodeJS" %}
1. Install the Portkey SDK in your application

{% code fullWidth="true" %}
```sh
npm i --save portkey-ai
```
{% endcode %}

2. Next, insert the Portkey-specific code as shown in the highlighted lines to your OpenAI completion calls. `PORTKEY_GATEWAY_URL` is portkey's gateway URL to route your requests and `createHeaders` is a convenience function that generates the headers object. ([All supported params/headers](../../../api-reference/portkey-sdk-client.md#nodejs-3))

<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai'; // We're using the v4 SDK
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'
</strong>
const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL,
<strong>  defaultHeaders: createHeaders({
</strong><strong>    provider: "openai",
</strong><strong>    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
</strong><strong>  })
</strong>});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-4-turbo',
  });

  console.log(chatCompletion.choices);
}

main();
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
1. Install the Portkey SDK in your application

```sh
pip install portkey-ai
```

2. Next, insert the Portkey-specific code as shown in the highlighted lines to your OpenAI function calls. `PORTKEY_GATEWAY_URL` is portkey's gateway URL to route your requests and `createHeaders` is a convenience function that generates the headers object. ([All supported params/headers](../../../api-reference/portkey-sdk-client.md#python-3))

<pre class="language-python"><code class="lang-python">from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
    api_key="OPENAI_API_KEY", # defaults to os.environ.get("OPENAI_API_KEY")
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        provider="openai",
</strong><strong>        api_key="PORTKEY_API_KEY" # defaults to os.environ.get("PORTKEY_API_KEY")
</strong><strong>    )
</strong>)

chat_complete = client.chat.completions.create(
    model="gpt-4-turbo",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/chat/completions \
</strong>  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
<strong>  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
</strong><strong>  -H "x-portkey-provider: openai" \ 
</strong>  -d '{
    "model": "gpt-4-turbo",
    "messages": [{
        "role": "system",
        "content": "You are a helpful assistant."
      },{
        "role": "user",
        "content": "Hello!"
      }]
  }'
</code></pre>

[List of all possible headers](../../../api-reference/portkey-sdk-client.md#rest-headers)
{% endtab %}
{% endtabs %}

This request will be automatically logged by Portkey. You can view this in your logs dashboard. Portkey logs the tokens utilized, execution time, and cost for each request. Additionally, you can delve into the details to review the precise request and response data.

### Track End-User IDs

Portkey allows you to track user IDs passed with the `user` parameter in OpenAI requests, enabling you to monitor user-level costs, requests, and more.

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-typescript"><code class="lang-typescript">const chatCompletion = await portkey.chat.completions.create({
  messages: [{ role: "user", content: "Say this is a test" }],
  model: "gpt-4o",
<strong>  user: "user_12345",
</strong>});
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">response = portkey.chat.completions.create(
  model="gpt-4o",
  messages=[{ role: "user", content: "Say this is a test" }]
<strong>  user="user_123456"
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

When you include the `user` parameter in your requests, Portkey logs will display the associated user ID, as shown in the image below:

<div align="left">

<figure><img src="../../../.gitbook/assets/CleanShot 2024-05-20 at 16.04.06@2x.png" alt="" width="375"><figcaption></figcaption></figure>

</div>

In addition to the `user` parameter, Portkey allows you to send arbitrary custom metadata with your requests. This powerful feature enables you to associate additional context or information with each request, which can be useful for analysis, debugging, or other custom use cases.

{% content-ref url="../../../product/observability/metadata.md" %}
[metadata.md](../../../product/observability/metadata.md)
{% endcontent-ref %}

{% hint style="info" %}
* The same integration approach applies to APIs for [`completions`](https://platform.openai.com/docs/guides/text-generation/completions-api), [`embeddings`](https://platform.openai.com/docs/api-reference/embeddings/create), [`vision`](https://platform.openai.com/docs/guides/vision/quick-start), [`moderation`](https://platform.openai.com/docs/api-reference/moderations/create), [`transcription`](https://platform.openai.com/docs/api-reference/audio/createTranscription), [`translation`](https://platform.openai.com/docs/api-reference/audio/createTranslation), [`speech`](https://platform.openai.com/docs/api-reference/audio/createSpeech) and [`files`](https://platform.openai.com/docs/api-reference/files/create).
* If you are looking for a way to add your **Org ID** & **Project ID** to the requests, head over to [this section](./#managing-openai-projects-and-organizations-in-portkey).
{% endhint %}

## Using the Prompts API

Portkey also supports creating and managing prompt templates in the [prompt library](../../../product/prompt-library.md). This enables the collaborative development of prompts directly through the user interface.

1. Create a prompt template with variables and set the hyperparameters.&#x20;

<figure><img src="../../../.gitbook/assets/prompt creation.gif" alt=""><figcaption></figcaption></figure>

2. Use this prompt in your codebase using the Portkey SDK.

{% tabs %}
{% tab title="Node" %}
```javascript
import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
})

// Make the prompt creation call with the variables
const promptCompletion = await portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    }
})
```

```javascript
// We can also override the hyperparameters
const promptCompletion = await portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    },
    max_tokens: 250,
    presence_penalty: 0.2
})
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

client = Portkey(
    api_key="PORTKEY_API_KEY",  # defaults to os.environ.get("PORTKEY_API_KEY")
)

prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    }
)

print(prompt_completion)

# We can also override the hyperparameters
prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    },
    max_tokens=250,
    presence_penalty=0.2
)
print(prompt_completion)
```
{% endtab %}

{% tab title="REST API" %}
```bash
curl -X POST "https://api.portkey.ai/v1/prompts/:PROMPT_ID/completions" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
    "variables": {
        # The variables to use
    },
    "max_tokens": 250, # Optional
    "presence_penalty": 0.2 # Optional
}'
```
{% endtab %}
{% endtabs %}

Observe how this streamlines your code readability and simplifies prompt updates via the UI without altering the codebase.

## Advanced Use Cases

### Streaming Responses

Portkey supports streaming responses using Server Sent Events (SSE).

{% tabs %}
{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    provider: "openai",
    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
  })
});

async function main() {
  const stream = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    stream: true,
</strong>  });
  for await (const chunk of stream) {
    process.stdout.write(chunk.choices[0]?.delta?.content || '');
  }
}

main();

</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key="OPENAI_API_KEY",  # defaults to os.environ.get("OPENAI_API_KEY")
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY" # defaults to os.environ.get("PORTKEY_API_KEY")
    )
)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
<strong>    stream=True
</strong>)

for chunk in chat_complete:
    print(chunk.choices[0].delta.content, end="", flush=True)

</code></pre>
{% endtab %}
{% endtabs %}

### Using Vision Models

Portkey's multimodal Gateway fully supports OpenAI vision models as well. See this guide for more info:

{% content-ref url="../../../product/ai-gateway/multimodal-capabilities/vision.md" %}
[vision.md](../../../product/ai-gateway/multimodal-capabilities/vision.md)
{% endcontent-ref %}

### Function Calling

Function calls within your OpenAI or Portkey SDK operations remain standard. These logs will appear in Portkey, highlighting the utilized functions and their outputs.

Additionally, you can define functions within your prompts and invoke the `portkey.prompts.completions.create` method as above.

### Fine-Tuning

Please refer to our fine-tuning guides to take advantage of Portkey's advanced [continuous fine-tuning](../../../product/autonomous-fine-tuning.md) capabilities.

### Image Generation

Portkey supports multiple modalities for OpenAI and you can make image generation requests through Portkey's AI Gateway the same way as making completion calls.

{% tabs %}
{% tab title="OpenAI NodeJS" %}
```javascript
// Define the OpenAI client as shown above

const image = await openai.images.generate({
  model:"dall-e-3",
  prompt:"Lucy in the sky with diamonds",
  size:"1024x1024"
})
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
# Define the OpenAI client as shown above

image = openai.images.generate(
  model="dall-e-3",
  prompt="Lucy in the sky with diamonds",
  size="1024x1024"
)
```
{% endtab %}
{% endtabs %}

Portkey's fast AI gateway captures the information about the request on your Portkey Dashboard. On your logs screen, you'd be able to see this request with the request and response.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Log view for an image generation request on OpenAI</p></figcaption></figure>

More information on image generation is available in the [API Reference](../../../provider-endpoints/images/create-image.md#create-image).

### Audio - Transcription, Translation, and Text-to-Speech

Portkey's multimodal Gateway also supports the `audio` methods on OpenAI API. Check out the below guides for more info:

{% content-ref url="../../../product/ai-gateway/multimodal-capabilities/text-to-speech.md" %}
[text-to-speech.md](../../../product/ai-gateway/multimodal-capabilities/text-to-speech.md)
{% endcontent-ref %}

{% content-ref url="../../../product/ai-gateway/multimodal-capabilities/vision-1.md" %}
[vision-1.md](../../../product/ai-gateway/multimodal-capabilities/vision-1.md)
{% endcontent-ref %}

***

## Managing OpenAI Projects & Organizations in Portkey

When integrating OpenAI with Portkey, you can specify your OpenAI organization and project IDs along with your API key. This is particularly useful if you belong to multiple organizations or are accessing projects through a legacy user API key.

Specifying the organization and project IDs helps you maintain better control over your access rules, usage, and costs.

In Portkey, you can add your Org & Project details by,

1. Creating your Virtual Key
2. Defining a Gateway Config
3. Passing Details in a Request

Let's explore each method in more detail.

### Using Virtual Keys

When selecting OpenAI from the dropdown menu while creating a virtual key, Portkey automatically displays optional fields for the organization ID and project ID alongside the API key field.&#x20;

[Get your OpenAI API key from here](https://platform.openai.com/api-keys), then add it to Portkey to create the virtual key that can be used throughout Portkey.

<figure><img src="../../../.gitbook/assets/CleanShot 2024-05-20 at 08.35.21@2x.png" alt=""><figcaption></figcaption></figure>

{% content-ref url="../../../product/ai-gateway/virtual-keys/" %}
[virtual-keys](../../../product/ai-gateway/virtual-keys/)
{% endcontent-ref %}

Portkey takes budget management a step further than OpenAI. While OpenAI allows setting budget limits per project, Portkey enables you to set budget limits for each virtual key you create. For more information on budget limits, refer to this documentation:

{% content-ref url="../../../product/ai-gateway/virtual-keys/budget-limits.md" %}
[budget-limits.md](../../../product/ai-gateway/virtual-keys/budget-limits.md)
{% endcontent-ref %}

### Using The Gateway Config

You can also specify the organization and project details in the gateway config, either at the root level or within a specific target.

<pre class="language-json"><code class="lang-json">{
	"provider": "openai",
	"api_key": "OPENAI_API_KEY"
<strong>	"openai_organization": "org-xxxxxx",
</strong><strong>	"openai_project": "proj_xxxxxxxx"
</strong>}
</code></pre>

### While Making a Request

You can also pass your organization and project details directly when making a request using curl, the OpenAI SDK, or the Portkey SDK.

{% tabs %}
{% tab title="OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key="OPENAI_API_KEY",
<strong>    organization="org-xxxxxxxxxx",
</strong><strong>    project="proj_xxxxxxxxx",
</strong>    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY"
    )
)

chat_complete = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)

</code></pre>
{% endtab %}

{% tab title="OpenAI TS SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";

const openai = new OpenAI({
  apiKey: "OPENAI_API_KEY",
<strong>  organization: "org-xxxxxx",
</strong><strong>  project: "proj_xxxxxxx",
</strong>  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    provider: "openai",
    apiKey: "PORTKEY_API_KEY",
  }),
});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: "user", content: "Say this is a test" }],
    model: "gpt-4o",
  });

  console.log(chatCompletion.choices);
}

main();

</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
<strong>  -H "x-portkey-openai-organization: org-xxxxxxx" \
</strong><strong>  -H "x-portkey-openai-project: proj_xxxxxxx" \
</strong>  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
</code></pre>
{% endtab %}

{% tab title="Portkey Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    provider="openai",
    authorization="Bearer OPENAI_API_KEY",
<strong>    openai_organization="org-xxxxxxxxx",
</strong><strong>    openai_project="proj_xxxxxxxxx",
</strong>)

chat_complete = portkey.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)

</code></pre>
{% endtab %}

{% tab title="Portkey Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: "PORTKEY_API_KEY",
  provider: "openai",
  Authorization: "Bearer OPENAI_API_KEY",
<strong>  openaiOrganization: "org-xxxxxxxxxxx",
</strong><strong>  openaiProject: "proj_xxxxxxxxxxxxx",
</strong>});

async function main() {
  const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: "user", content: "Say this is a test" }],
    model: "gpt-4o",
  });

  console.log(chatCompletion.choices);
}

main();

</code></pre>
{% endtab %}
{% endtabs %}

***

### Portkey Features

Portkey supports the complete host of it's functionality via the OpenAI SDK so you don't need to migrate away from it.

Please find more information in the relevant sections:

1. [Add metadata to your requests](../../../product/observability/metadata.md)
2. [Add gateway configs to the OpenAI client or a single request](../../../product/ai-gateway/configs.md)
3. [Tracing OpenAI requests](../../../product/observability/traces.md)
4. [Setup a fallback to Azure OpenAI](../../../product/ai-gateway/fallbacks.md)
