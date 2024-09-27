# Google Vertex AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Google Vertex AI](https://cloud.google.com/vertex-ai?hl=en).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`vertex-ai`**</mark>
{% endhint %}

## Portkey SDK Integration with Google Vertex AI

Portkey provides a consistent API to interact with models from various providers. To integrate Google Vertex AI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Google Vertex AI API through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```bash
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

To integrate Vertex AI with Portkey, you'll need your **`Vertex Project Id`** & **`Vertex Region`**, with which you can set up the Virtual key.

[Here's a guide on how to find your Vertex Project details](vertex-ai.md#how-to-find-your-google-vertex-credentials).&#x20;

If you are integrating through Service Account File, [refer to this guide](vertex-ai.md#get-your-vertex-service-account-json).

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "VERTEX_VIRTUAL_KEY", // Your Vertex AI Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="VERTEX_VIRTUAL_KEY"   # Replace with your virtual key for Google
</strong>)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";

const portkey = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
<strong>    virtualKey: "PORTKEY_VERTEX_VIRTUAL_KEY",
</strong><strong>    authorization: "Bearer $GCLOUD AUTH PRINT-ACCESS-TOKEN"
</strong>  }),
});
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you do not want to add your Vertex AI details to Portkey vault, you can directly pass them while instantiating the Portkey client. [More on that here](vertex-ai.md#making-requests-without-virtual-keys).
{% endhint %}

### **3. Invoke Chat Completions with** Vertex AI and Gemini&#x20;

Use the Portkey instance to send requests to Gemini models hosted on Vertex AI. You can also override the virtual key directly in the API call if needed.

{% hint style="warning" %}
Vertex AI uses OAuth2 to authenticate its requests, so you need to send the **access token** additionally along with the request.
{% endhint %}

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gemini-pro',
<strong>}, {authorization: "vertex ai access token here"});
</strong>
console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python"><strong>completion = portkey.with_options(authorization="...").chat.completions.create(
</strong>    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'gemini-pro'
)

print(completion)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
```typescript
async function main() {
  const response = await portkey.chat.completions.create({
    messages: [{ role: "user", content: "1729" }],
    model: "gemini-1.5-flash-001",
    max_tokens: 128,
  });
  console.log(response.choices[0].message.content);
}

main();
```
{% endtab %}
{% endtabs %}

***

## Document, Video, Audio Processing

Vertex AI supports attaching `mp4`, `pdf`, `jpg`, `mp3`, `wav`, etc. file types to your Gemini messages.

{% hint style="info" %}
Gemini Docs:

* [Document Processing](https://ai.google.dev/gemini-api/docs/document-processing?lang=python)
* [Video & Image Processing](https://ai.google.dev/gemini-api/docs/vision?lang=python)
* [Audio Processing](https://ai.google.dev/gemini-api/docs/audio?lang=python)
{% endhint %}

Using Portkey, here's how you can send these media files:

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [
        { role: 'system', content: 'You are a helpful assistant' },
        { role: 'user', content: [
            {
                type: 'image_url',
                image_url: {
                    url: 'gs://cloud-samples-data/generative-ai/image/scones.jpg'
                }
            },
            {
                type: 'text',
                text: 'Describe the image'
            }
        ]}
    ],
    model: 'gemini-1.5-pro-001',
    max_tokens: 200
});
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages=[
        {
            "role": "system",
            "content": "You are a helpful assistant"
        },
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "gs://cloud-samples-data/generative-ai/image/scones.jpg"
                    }
                },
                {
                    "type": "text",
                    "text": "Describe the image"
                }
            ]
        }
    ],
    model='gemini-1.5-pro-001',
    max_tokens=200
)

print(completion)
```
{% endtab %}

{% tab title="Curl" %}
```sh
curl --location 'https://api.portkey.ai/v1/chat/completions' \
--header 'x-portkey-provider: vertex-ai' \
--header 'x-portkey-vertex-region: us-central1' \
--header 'Content-Type: application/json' \
--header 'x-portkey-api-key: PORTKEY_API_KEY' \
--header 'Authorization: GEMINI_API_KEY' \
--data '{
    "model": "gemini-1.5-pro-001",
    "max_tokens": 200,
    "stream": false,
    "messages": [
        {
            "role": "system",
            "content": "You are a helpful assistant"
        },
        {
            "role": "user",
            "content": [
                {    
                    "type": "image_url",
                    "image_url": {
                        "url": "gs://cloud-samples-data/generative-ai/image/scones.jpg"
                    }
                },
                {
                    "type": "text",
                    "text": "describe this image"
                }
            ]
        }
    ]
}'
```
{% endtab %}
{% endtabs %}

This same message format also works for all other media types — just send your media file in the `url` field, like <mark style="color:green;">`"url": "gs://cloud-samples-data/video/animals.mp4"`</mark>

### Sending `base64` Image

Here, you can send the `base64` image data along with the `url` field too:&#x20;

```json
"url": "data:image/png;base64,UklGRkacAABXRUJQVlA4IDqcAAC....."
```

***

## Function Calling

Portkey supports function calling mode on Google's Gemini Models. Explore this :arrow\_down: Cookbook for a deep dive and examples:

{% content-ref url="../../guides/getting-started/function-calling.md" %}
[function-calling.md](../../guides/getting-started/function-calling.md)
{% endcontent-ref %}

## Managing Vertex AI Prompts

You can manage all prompts to Google Gemini in the [Prompt Library](../../product/prompt-library.md). All the models in the model garden are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Making Requests Without Virtual Keys <a href="#making-requests-without-virtual-keys" id="making-requests-without-virtual-keys"></a>

You can also pass your Vertex AI details & secrets directly without using the Virtual Keys in Portkey.

Vertex AI expects a **`region`**, a **`project ID`** and the **`access token`** in the request for a successful completion request. This is how you can specify these fields directly in your requests:

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    vertexProjectId: "sample-55646",
</strong><strong>    vertexRegion: "us-central1",
</strong><strong>    provider:"vertex_ai",
</strong><strong>    Authorization: "$GCLOUD AUTH PRINT-ACCESS-TOKEN"
</strong>})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gemini-pro',
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    vertex_project_id="sample-55646",
</strong><strong>    vertex_region="us-central1",
</strong><strong>    provider="vertex_ai",
</strong><strong>    Authorization="$GCLOUD AUTH PRINT-ACCESS-TOKEN"
</strong>)

completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'gemini-pro'
)

print(completion)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";

const portkey = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
<strong>    provider: "vertex-ai",
</strong><strong>    vertexRegion: "us-central1",
</strong><strong>    vertexProjectId: "xxx"
</strong><strong>    Authorization: "Bearer $GCLOUD AUTH PRINT-ACCESS-TOKEN",
</strong>    // forwardHeaders: ["authorization"] // You can also directly forward the auth token to Google
  }),
});

async function main() {
  const response = await portkey.chat.completions.create({
    messages: [{ role: "user", content: "1729" }],
    model: "gemini-1.5-flash-001",
    max_tokens: 32,
  });
  console.log(response.choices[0].message.content);
}

main();
</code></pre>
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl 'https://api.portkey.ai/v1/chat/completions' \
-H 'Content-Type: application/json' \
-H 'x-portkey-api-key: PORTKEY_API_KEY' \
<strong>-H 'x-portkey-provider: vertex-ai' \
</strong><strong>-H 'Authorization: Bearer VERTEX_AI_ACCESS_TOKEN' \
</strong><strong>-H 'x-portkey-vertex-project-id: sample-94994' \
</strong><strong>-H 'x-portkey-vertex-region: us-central1' \
</strong>--data '{
    "messages": [{"role": "user","content": "Hello"}],
    "max_tokens": 20,
    "model": "gemini-pro"
}'
</code></pre>
{% endtab %}
{% endtabs %}

For further questions on custom Vertex AI deployments or fine-grained access tokens, reach out to us on support@portkey.ai

***

## How to Find Your Google Vertex Project Details

To obtain your **Vertex Project ID and Region,** [navigate to Google Vertex Dashboard](https://console.cloud.google.com/vertex-ai).&#x20;

* You can copy the **Project ID** located at the top left corner of your screen.&#x20;
* Find the **Region dropdown** on the same page to get your Vertex Region.

<figure><img src="../../.gitbook/assets/Portkey Group (7).png" alt=""><figcaption></figcaption></figure>

### **Get Your Vertex Service Account JSON**

* [Follow this process](https://cloud.google.com/iam/docs/keys-create-delete) to get your Service Account JSON.

***

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your Vertex AI requests](../../product/ai-gateway/configs.md)
3. [Tracing Vertex AI requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to Vertex AI APIs](../../product/ai-gateway/fallbacks.md)
