# AI Gateway

Portkey providers the world's fastest multimodal AI gateway. Seamlessly integrate multiple LLM providers and modalities while enhancing your app's reliability, cost-efficiency, and accuracy.

### **Features**

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h3>Universal API</h3></td><td>Use any of the supported models with a <strong>universal API</strong> (REST and SDKs)</td><td><a href="../../.gitbook/assets/10.png">10.png</a></td><td><a href="universal-api.md">universal-api.md</a></td></tr><tr><td><h3>Cache (Simple &#x26; Semantic)</h3></td><td>Save costs and decrease latencies by using a <strong>cache</strong></td><td><a href="../../.gitbook/assets/13.png">13.png</a></td><td><a href="cache-simple-and-semantic.md">cache-simple-and-semantic.md</a></td></tr><tr><td><h3>Fallbacks</h3></td><td><strong>Fallback</strong> between providers and models for resilience</td><td><a href="../../.gitbook/assets/12 (1).png">12 (1).png</a></td><td><a href="fallbacks.md">fallbacks.md</a></td></tr><tr><td><h3>Multimodality</h3></td><td>Use vision, audio, image generation, and more models</td><td><a href="../../.gitbook/assets/multimoda.png">multimoda.png</a></td><td><a href="multimodal-capabilities/">multimodal-capabilities</a></td></tr><tr><td><h3>Automatic Retries</h3></td><td>Setup <strong>automatic retry</strong> strategies</td><td><a href="../../.gitbook/assets/14.png">14.png</a></td><td><a href="automatic-retries.md">automatic-retries.md</a></td></tr><tr><td><h3>Load Balancing</h3></td><td><strong>Load balance</strong> between various API Keys to counter rate-limits</td><td><a href="../../.gitbook/assets/11.png">11.png</a></td><td><a href="load-balancing.md">load-balancing.md</a></td></tr><tr><td><h3>Canary Testing</h3></td><td><strong>Canary test</strong> new models in production</td><td><a href="../../.gitbook/assets/11.png">11.png</a></td><td><a href="canary-testing.md">canary-testing.md</a></td></tr><tr><td><h3>Vault</h3></td><td>Manage AI provider keys in a secure <strong>vault</strong></td><td><a href="../../.gitbook/assets/Untitled design (9).png">Untitled design (9).png</a></td><td><a href="virtual-keys/">virtual-keys</a></td></tr><tr><td><h3>Request Timeout</h3></td><td>Easily handle unresponsive LLM requests</td><td><a href="../../.gitbook/assets/request_timeout.png">request_timeout.png</a></td><td><a href="request-timeouts.md">request-timeouts.md</a></td></tr><tr><td><h3>Conditional Routing</h3></td><td>Route to different targets based on custom conditional checks</td><td></td><td><a href="conditional-routing.md">conditional-routing.md</a></td></tr></tbody></table>

### Using the Gateway

The various gateway strategies are implemented using Gateway configs. You can read more about configs below.

{% content-ref url="configs.md" %}
[configs.md](configs.md)
{% endcontent-ref %}

## Open Source

We've open sourced our battle-tested AI gateway to the community. You can run it locally with a single command:

```bash
npx @portkey-ai/gateway
```

[**Contribute here**](https://github.com/portkey-ai/gateway).

{% hint style="success" %}
While you're here, why not [give us a star](https://git.new/ai-gateway-docs)? It helps us a lot!
{% endhint %}

<figure><img src="../../.gitbook/assets/Rubeus Social Share (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You can also [self-host](https://github.com/Portkey-AI/gateway/blob/main/docs/installation-deployments.md) the gateway and then connect it to Portkey. Please reach out on hello@portkey.ai and we'll help you set this up!
{% endhint %}
