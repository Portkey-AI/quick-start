---
description: Use Portkey with Llama Agents to take your AI Agents to production
---

# LlamaIndex

## Getting Started

### 1. Install the required packages:

```bash
pip install -qU llama-agents llama-index portkey-ai
```

### &#x20;2. Configure your Llama Index LLM objects:

```python
from llama_index.llms.openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders


gpt_4o_config = {
    "provider": "openai", #Use the provider of choice
    "api_key": "YOUR_OPENAI_KEY,
    "override_params": { "model":"gpt-4o" }
}

gpt_4o = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key=userdata.get('PORTKEY_API_KEY'),
        config=gpt_4o_config
    )
)

```

That's all you need to do to use Portkey with Llama Index agents. Execute your agents and visit [Portkey.ai](https://portkey.ai) to observe your Agent's activity.

## Integration Guide

Here's a simple Google Colab notebook that demonstrates Llama Index with Portkey integration

[![Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://git.new/llama-agents)

{% embed url="https://www.loom.com/share/bd85a22342c8408cbefeba1c1be20371?sid=1843f659-b83e-4480-9470-caf0683fd32d" %}

## Make your agents Production-ready with Portkey

Portkey makes your Llama Index agents reliable, robust, and production-grade with its observability suite and AI Gateway. Seamlessly integrate 200+ LLMs with your Llama Index agents using Portkey. Implement fallbacks, gain granular insights into agent performance and costs, and continuously optimize your AI operations—all with just 2 lines of code.

Let's dive deep! Let's go through each of the use cases!

### 1. [Interoperability](../../product/ai-gateway/universal-api.md)

Easily switch between 200+ LLMs. Call various LLMs such as Anthropic, Gemini, Mistral, Azure OpenAI, Google Vertex AI,  AWS Bedrock, and many more by simply changing the  `provider` and `API key` in the `ChatOpenAI` object.

{% tabs %}
{% tab title="OpenAI to Azure OpenAI" %}
If you are using OpenAI with CrewAI, your code would look like this:

```python
llm_config = {
    "provider": "openai", #Use the provider of choice
    "api_key": "YOUR_OPENAI_KEY,
    "override_params": { "model":"gpt-4o" }
}


llm = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        config=llm_config
    )
)
```

To switch to Azure as your provider, add your Azure details to Portley vault ([here's how](../llms/azure-openai.md)) and use Azure OpenAI using virtual keys

```python
llm_config = {
    provider="azure-openai", #choose your provider
    "api_key": "YOUR_OPENAI_KEY,
    "override_params": { "model":"gpt-4o" }
}

llm = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        config=llm_config
    )
)
```
{% endtab %}

{% tab title="Anthropic to AWS Bedrock" %}
If you are using Anthropic with CrewAI, your code would look like this:

```python
llm_config = {
    "provider": "anthropic", #Use the provider of choice
    "api_key": "YOUR_OPENAI_KEY,
    "override_params": { "model":"claude-3-5-sonnet-20240620" }
}


llm = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        config=llm_config
    )
)
```

To switch to AWS Bedrock as your provider, add your AWS Bedrock details to Portley vault ([here's how](../llms/aws-bedrock.md)) and use AWS Bedrock using virtual keys,

```python
llm_config = {
    "provider": "bedrock", #Use the provider of choice
    "api_key": "YOUR_AWS_KEY",
    "override_params": { "model":"gpt-4o" }
}

llm = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        config=llm_config
    )
)
```
{% endtab %}
{% endtabs %}

### 2. [Reliability](../../product/ai-gateway/)

Agents are _brittle_. Long agentic pipelines with multiple steps can fail at any stage, disrupting the entire process. Portkey solves this by offering built-in **fallbacks** between different LLMs or providers, **load-balancing** across multiple instances or API keys, and implementing automatic **retries** and request **timeouts**. This makes your agents more reliable and resilient.

Here's how you can implement these features using Portkey's config

```json
{
  "retry": {
    "attempts": 5
  },
  "strategy": {
    "mode": "loadbalance" // Choose between "loadbalance" or "fallback"
  },
  "targets": [
    {
      "provider": "openai",
      "api_key": "OpenAI_API_Key"
    },
    {
      "provider": "anthropic",
      "api_key": "Anthropic_API_Key"
    }
  ]
}
```

### 3. [Metrics](../../product/observability/)

Agent runs can be costly. Tracking agent metrics is crucial for understanding the performance and reliability of your AI agents. Metrics help identify issues, optimize runs, and ensure that your agents meet their intended goals.

Portkey automatically logs comprehensive metrics for your AI agents, including **cost**, **tokens used**, **latency**, etc. Whether you need a broad overview or granular insights into your agent runs, Portkey's customizable filters provide the metrics you need.\
For agent-specific observability, add `Trace-id` to the request headers for each agent.&#x20;

```python
llm2 = ChatOpenAI(
    api_key="Anthropic_API_Key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        provider="anthropic",
        trace_id="research_agent1" #Add individual trace-id for your agent analytics
    )
)
```

### 4. [Logs](../../product/observability/logs.md)

Agent runs are complex. Logs are essential for diagnosing issues, understanding agent behavior, and improving performance. They provide a detailed record of agent activities and tool use, which is crucial for debugging and optimizing processes.

Portkey offers comprehensive logging features that capture detailed information about every action and decision made by your AI agents. Access a dedicated section to view records of agent executions, including parameters, outcomes, function calls, and errors. Filter logs based on multiple parameters such as trace ID, model, tokens used, and metadata.

<figure><img src="../../.gitbook/assets/222.gif" alt=""><figcaption></figcaption></figure>

### 5. [Traces](../../product/observability/traces.md)

With traces, you can see each agent run granularly on Portkey. Tracing your LlamaIndex agent runs helps in debugging, performance optimzation, and visualizing how exactly your agents are running.

### Using Traces in LlamaIndex Agents

#### Step 1: Import & Initialize the Portkey LlamaIndex Callback Handler

<pre class="language-python"><code class="lang-python">from portkey_ai.llamaindex import LlamaIndexCallbackHandler

portkey_handler = LlamaIndexCallbackHandler(
<strong>    api_key="YOUR_PORTKEY_API_KEY",
</strong><strong>    metadata={
</strong><strong>        "session_id": "session_1",  # Use consistent metadata across your application
</strong><strong>        "agent_id": "research_agent_1",  # Specific to the current agent
</strong><strong>    }
</strong>)


</code></pre>

#### Step 2: Configure Your LLM with the Portkey Callback

```python
from llama_index.llms.openai import OpenAI

llm = OpenAI(
    api_key="YOUR_OPENAI_API_KEY_HERE",
    callbacks=[portkey_handler],  # Replace with your OpenAI API key
    # ... other parameters
)
```

With Portkey tracing, you can encapsulate the complete execution of your agent workflow.

### 6. [Continuous Improvement](../../product/observability/feedback.md)

Improve your Agent runs by capturing qualitative & quantitative user feedback on your requests.\
Portkey's Feedback APIs provide a simple way to get weighted feedback from customers on any request you served, at any stage in your app. You can capture this feedback on a request or conversation level and analyze it by adding meta data to the relevant request.

### 7. [Caching](../../product/ai-gateway/cache-simple-and-semantic.md)

Agent runs are time-consuming and expensive due to their complex pipelines. Caching can significantly reduce these costs by storing frequently used data and responses.\
Portkey offers a built-in caching system that stores past responses, reducing the need for agent calls saving both time and money.

```json
{
 "cache": {
    "mode": "semantic" // Choose between "simple" or "semantic"
 }
}
```

### 8. [Security & Compliance](../../product/enterprise-offering/security-portkey.md)

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both the app and the Portkey APIs.

***

## [Portkey Config](../../product/ai-gateway/configs.md)

Many of these features are driven by Portkey's Config architecture. The Portkey app simplifies creating, managing, and versioning your Configs.

For more information on using these features and setting up your Config, please refer to the [Portkey documentation](https://docs.portkey.ai).
