# Autonomous Fine-tuning

{% hint style="info" %}
**This feature is in private beta.**\
Please drop us a message on support@portkey.ai or on our [Discord](https://discord.gg/DD7vgKK299) if you're interested.
{% endhint %}

### What is Autonomous LLM Fine-tuning?

Autonomous Fine-tuning is a powerful feature offered by Portkey AI that enables organizations to automatically create, manage, and execute fine-tuning jobs for Large Language Models (LLMs) across multiple providers.&#x20;

This feature leverages your existing API usage data to continuously improve and customize LLM performance for your specific use cases.

### Benefits

* **Automated Workflow**: Streamline the entire fine-tuning process from data preparation to model deployment.
* **Multi-Provider Support**: Fine-tune models across 10+ providers, including OpenAI, Azure, AWS Bedrock, and Anyscale.
* **Data-Driven Improvements**: Utilize your actual API usage data to create relevant and effective fine-tuning datasets.
* **Continuous Learning**: Set up periodic fine-tuning jobs to keep your models up-to-date with the latest data.
* **Enhanced Performance**: Improve model accuracy and relevance for your specific use cases.
* **Cost-Effective**: Optimize your LLM usage by fine-tuning models to better suit your needs, potentially reducing the number of API calls required.
* **Centralized Management**: Manage all your fine-tuning jobs across different providers from a single interface.

### Data Preparation

1. **Log Collection**: Portkey's AI gateway automatically collects and stores logs from your LLM API requests.
2. **Data Enrichment**:
   * Filter logs based on various criteria.
   * Annotate logs with additional information.
   * Use Portkey's Guardrails feature for automatic log annotation.
3. **Dataset Creation**: Utilize filters to select the most relevant logs for your fine-tuning dataset.
4. **Data Export**: Export the enriched logs as a dataset suitable for fine-tuning.

### Fine-tuning Process

1. **Model Selection**: Choose from a wide range of supported LLM providers and models.
2. **Job Configuration**: Set up fine-tuning parameters through an intuitive UI.
3. **Execution**: Portkey triggers the fine-tuning job on the selected provider's platform.
4. **Monitoring**: Track the progress of your fine-tuning jobs through Portkey's dashboard.
5. **Deployment**: Once complete, the fine-tuned model becomes available for use through Portkey's API gateway.

### How It Works: Step-by-Step

1. **Data Collection**: As you use Portkey's AI gateway for LLM requests, logs are automatically collected and stored in your Portkey account.
2. **Data Enrichment**:
   * Apply filters to your log data.
   * Add annotations and additional context to logs.
   * Utilize Portkey's Guardrails feature for automatic input/output annotations.
3. **Dataset Creation**:
   * Use the enriched log data to create a curated dataset for fine-tuning.
   * Apply additional filters to select the most relevant data points.
4. **Fine-tuning Job Setup**:
   * Access the Fine-tuning feature in Portkey's UI.
   * Select your desired LLM provider and model.
   * Choose your prepared dataset.
   * Configure fine-tuning parameters.
5. **Job Execution**:
   * Portkey initiates the fine-tuning job on the chosen provider's platform.
   * Monitor the progress through Portkey's dashboard.
6. **Model Deployment**:
   * Once fine-tuning is complete, the new model becomes available through Portkey's API gateway.
7. **Continuous Improvement** (Optional):
   * Set up periodic fine-tuning jobs (daily, weekly, or monthly).
   * Portkey automatically creates and executes these jobs using the latest data.

### Partnerships

Portkey AI has established partnerships to extend the capabilities of its Autonomous Fine-tuning feature:

* **OpenPipe**: Integration allows Portkey's enriched data to be used on OpenPipe's fine-tuning platform.
* **Pipeshift**: Portkey's datasets can be seamlessly utilized in Pipeshift's inference platform.

These partnerships enable users to leverage Portkey's data preparation capabilities with specialized fine-tuning and inference services.

### Getting Started

To begin using Autonomous Fine-tuning:

1. Ensure you have an active Portkey AI account with the AI gateway set up.
2. Navigate to the Fine-tuning section in your Portkey dashboard.
3. Follow the step-by-step wizard to create your first fine-tuning job.
4. For assistance, consult our detailed documentation or contact Portkey support.

### Best Practices

* Regularly review and update your data filtering criteria to ensure the quality of your fine-tuning datasets.
* Start with smaller, focused datasets before scaling up to larger fine-tuning jobs.
* Monitor the performance of your fine-tuned models and iterate as needed.
* Leverage Portkey's analytics to gain insights into your model's performance improvements.

By utilizing Portkey AI's Autonomous Fine-tuning feature, you can harness the power of your own data to create customized, high-performing LLMs tailored to your specific needs, all while streamlining the management of multiple AI providers.

### API Support

{% content-ref url="../provider-endpoints/fine-tuning/" %}
[fine-tuning](../provider-endpoints/fine-tuning/)
{% endcontent-ref %}
