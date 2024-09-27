# Deepseek

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including DeepSeek models.

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**:  **<mark style="color:blue;">**deepseek**</mark>
{% endhint %}

## Portkey SDK Integration with DeepSeek Models

Portkey provides a consistent API to interact with models from various providers. To integrate DeepSeek with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with DeepSeek AI's API through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```python
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

To use DeepSeek with Portkey, [get your API key from here](https://platform.deepseek.com/api\_keys), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your DeepSeek Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Groq
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** DeepSeek

Use the Portkey instance to send requests to DeepSeek. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'deepseek-chat',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'mistral-medium'
)

print(completion)
```
{% endtab %}
{% endtabs %}

## Managing DeepSeek Prompts

You can manage all prompts to DeepSeek in the [Prompt Library](../../product/prompt-library.md). All the current models of DeepSeek are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.&#x20;

### Supported Endpoints

1. <mark style="color:blue;">`CHAT_COMPLETIONS`</mark>
2. <mark style="color:blue;">`STREAM_CHAT_COMPLETIONS`</mark>

The complete list of features supported in the SDK is available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your DeepSeek](../../product/ai-gateway/configs.md)[ requests](../../product/ai-gateway/configs.md)
3. [Tracing DeepSeek requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to DeepSeek APIs](../../product/ai-gateway/fallbacks.md)
