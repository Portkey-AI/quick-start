# Google Gemini

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Google Gemini APIs](https://cloud.google.com/vertex-ai/docs/generative-ai/model-reference/gemini).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`google`**</mark>
{% endhint %}

## Portkey SDK Integration with Google Gemini Models

Portkey provides a consistent API to interact with models from various providers. To integrate Google Gemini with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Google Gemini's API through Portkey's gateway.

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

To use Gemini with Portkey, [get your API key from here](https://aistudio.google.com/app/apikey), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Google Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Google
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** Google Gemini&#x20;

Use the Portkey instance to send requests to Google Gemini. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [
        { role: 'system', content: 'You are not a helpful assistant' },
        { role: 'user', content: 'Say this is a test' }
    ],
    model: 'gemini-1.5-pro',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [
        { "role": 'system', "content": 'You are not a helpful assistant' },
        { "role": 'user', "content": 'Say this is a test' }
    ],
    model= 'gemini-1.5-pro'
)

print(completion)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Portkey supports the `system_instructions` parameter for Google Gemini 1.5 - allowing you to control the behavior and output of your Gemini-powered applications with ease.&#x20;



Simply include your Gemini system prompt as part of the <mark style="color:green;">`{"role":"system"}`</mark> message within the `messages` array of your request body. Portkey Gateway will automatically transform your message to ensure seamless compatibility with the Google Gemini API.
{% endhint %}

## Document, Video, Audio Processing with Gemini

Gemini supports attaching `mp4`, `pdf`, `jpg`, `mp3`, `wav`, etc. file types to your messages.

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

## Function Calling

Portkey supports function calling mode on Google's Gemini Models. Explore this :arrow\_down: Cookbook for a deep dive and examples:

{% content-ref url="../../guides/getting-started/function-calling.md" %}
[function-calling.md](../../guides/getting-started/function-calling.md)
{% endcontent-ref %}

## Managing Google Gemini Prompts

You can manage all prompts to Google Gemini in the [Prompt Library](../../product/prompt-library.md). All the current models of Google Gemini are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your Gemini requests](../../product/ai-gateway/configs.md)
3. [Tracing Google Gemini requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to Gemini APIs](../../product/ai-gateway/fallbacks.md)
