# Voyage AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including Voyage AI's embedding and Re-rank endpoints.

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**voyage**</mark>
{% endhint %}

## Portkey SDK Integration with Voyage

Portkey provides a consistent API to interact with models from Voyage. To integrate Voyage with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Voyage AI's models through Portkey's gateway.

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

### **2. Initialize Portkey with the API Key**

To use Voyage with Portkey, [get your API key from here](https://dash.voyageai.com/), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    Authorization: "VOYAGE_API_KEY",  // Replace with your Voyage API key
    provider: "voyage"
    
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    Authorization="VOYAGE_API_KEY",  # Replace with your Voyage API key
    provider="voyage"
)
```
{% endtab %}
{% endtabs %}

### Embeddings

Embedding endpoints are natively supported within Portkey like this:

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const embedding = await portkey.embeddings.create({
    input: 'Name the tallest buildings in Hawaii',
    model: 'voyage-3'
});

console.log(embedding);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
embedding =  portkey.embeddings.create(
    input= 'Name the tallest buildings in Hawaii',
    model= 'voyage-3'
)

print(embedding)
```
{% endtab %}
{% endtabs %}

### Re-ranking

You can use Voyage reranking the `portkey.post` method with the body expected by [C](https://docs.cohere.com/reference/rerank-1)

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const response = await portkey.post(
"/rerank",
{
  "model": "rerank-lite-1",
  "query": "What is the capital of the United States?",
  "documents": [
    "Carson City is the capital city of the American state of Nevada.",
    "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
    "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
    "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
  ]
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
response = portkey.post(
        "/rerank", 
        model="rerank-lite-1",
        query="What is the capital of the United States?",
        documents=[
            "Carson City is the capital city of the American state of Nevada.",
            "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
            "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
            "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
        ]
    )

print(response)
```
{% endtab %}
{% endtabs %}

## Next Steps

The complete list of features supported in the SDK is available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

\
You'll find more information in the relevant sections:

1. [Add metadata to your requests](https://docs.portkey.ai/docs/product/observability/metadata)
2. [Add gateway configs to your Voyage requests](https://docs.portkey.ai/docs/product/ai-gateway/configs)
3. [Tracing Voyage requests](https://docs.portkey.ai/docs/product/observability/traces)
4. [Setup a fallback from OpenAI to Voyage APIs](https://docs.portkey.ai/docs/product/ai-gateway/fallbacks)

[\
](https://docs.portkey.ai/docs/integrations/llms/cerebras)
