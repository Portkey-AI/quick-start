# LibreChat

LibreChat is an "[enhanced ChatGPT clone](https://github.com/danny-avila/LibreChat)". You can deploy it internally (or externally) to instantly spin up a Chat UI for any of your AI use cases.

Portkey **natively integrates** with LibreChat and makes your LibreChat deployments **production-grade** and **reliable** with our suite of features:

* Full-stack **observability** and **tracing** for all requests
* Interoperability across **250+ LLMs**
* Built-in **50+** state-of-the-art guardrails
* Simple & semantic **caching** to save costs & time
* Conditional request **routing** with **fallbacks**, **load-balancing**, automatic **retries**, and more
* Continuous improvement based on user **feedback**

***

## Integrate Portkey with LibreChat

### Step 1. Create the `docker-compose-override.yaml` file, [following the instructions here](https://www.librechat.ai/docs/quick\_start/custom\_endpoints).

This file will point to the `librechat.yaml` file where we will configure our Portkey settings (in Step 3).

{% tabs %}
{% tab title="docker-compose.override.yml" %}
```yaml
services:
  api:
    volumes:
    - type: bind
      source: ./librechat.yaml
      target: /app/librechat.yaml
```
{% endtab %}
{% endtabs %}

### Step 2: Configure the `.env` file

Edit your existing `.env` file at the project root (if the file does not exist, copy the `.env.example` file and rename to `.env`). We will add:

{% tabs %}
{% tab title=".env" %}
```
PORTKEY_API_KEY=YOUR_PORTKEY_API_KEY
PORTKEY_BASE_URL=https://api.portkey.ai/v1
```
{% endtab %}
{% endtabs %}

### Step 3: Edit the `librechat.yaml` file, [following the instructions here](https://www.librechat.ai/docs/quick\_start/custom\_endpoints).

Here, you can either pass your **Config** (containing provider/model configurations) or direct provider **Virtual key** saved on Portkey.

{% tabs %}
{% tab title="librechat.yaml with Portkey Virtual Key " %}
<pre class="language-yaml"><code class="lang-yaml">version: 1.1.4
cache: true
endpoints:
  custom:
    - name: "Portkey"
<strong>      baseURL: ${PORTKEY_GATEWAY_URL}
</strong>      headers:
<strong>        x-portkey-api-key: "${PORTKEY_API_KEY}"
</strong><strong>        x-portkey-virtual-key: "PORTKEY_OPENAI_VIRTUAL_KEY"
</strong>      models:
        default: ["gpt-4o-mini"]
        fetch: true
      titleConvo: true
      titleModel: "current_model"
      summarize: false
      summaryModel: "current_model"
      forcePrompt: false
      modelDisplayLabel: "Portkey:OpenAI"
</code></pre>
{% endtab %}

{% tab title="librechat.yaml with Portkey Config" %}
<pre class="language-yaml"><code class="lang-yaml">version: 1.1.4
cache: true
endpoints:
  custom:
    - name: "Portkey"
<strong>      baseURL: ${PORTKEY_GATEWAY_URL}
</strong>      headers:
<strong>        x-portkey-api-key: "${PORTKEY_API_KEY}"
</strong><strong>        x-portkey-config: "pc-libre-xxx"
</strong>      models:
        default: ["llama-3.2"]
        fetch: true
      titleConvo: true
      titleModel: "current_model"
      summarize: false
      summaryModel: "current_model"
      forcePrompt: false
      modelDisplayLabel: "Portkey:Llama"
</code></pre>
{% endtab %}
{% endtabs %}

***

## Support for the LibreChat Integration

If you face any issues in integrating Portkey with LibreChat, please ping the Portkey team on our commuinty forum [here](https://portkey.wiki/community).
