# MongoDB

{% hint style="success" %}
This integration is available for [Portkey Enterprise](../../) users.
{% endhint %}

When deploying your AI app to production, having a robust, scalable, and high-performance logging solution is crucial. That's where Portkey and MongoDB combine — Portkey's Enterprise version easily lets you store all your LLM logs on the most popular database services of your choice. This is made even easier with our ready-to-use Kubernetes Configs (Helm charts).

> Portkey is part of MongoDB partner ecosystem to help you build & deploy your AI apps with confidence. [Learn more](https://cloud.mongodb.com/ecosystem/portkey-ai)

## Getting Started with MongoDB Log Storage

#### Prerequisites

* Portkey Enterprise account
* Access to a MongoDB instance
* Kubernetes cluster

{% hint style="info" %}
[**Link to Helm Charts repo**](https://github.com/Portkey-AI/helm-chart/tree/main/helm/enterprise)
{% endhint %}

#### Configuration

To use MongoDB for Log storage, you'll need to provide the following values in your `values.yaml` file for the Helm chart deployment:

```yaml
  MONGO_DB_CONNECTION_URL: 
  MONGO_DATABASE: 
  MONGO_COLLECTION_NAME:
  MONGO_GENERATION_HOOKS_COLLECTION_NAME
```

#### Authentication with PEM File

If you're using a PEM file for authentication, follow these additional steps:

1. In the `resources-config.yaml` file, supply PEM file details under the `data` section:

```yaml
data:
  document_db.pem: |
    -----BEGIN CERTIFICATE-----
    Your certificate content here
    -----END CERTIFICATE-----
```

2. In `values.yaml`, add the following configuration:

```yaml
volumes:
- name: shared-folder
  configMap:
    name: resource-config
volumeMounts:
- name: shared-folder
  mountPath: /etc/shared/<shared_pem>
  subPath: <shared_pem>
```

3. Update your `MONGO_DB_CONNECTION_URL` to use the PEM file:

```bash
mongodb://<user>:<password>@<host>?tls=true&tlsCAFile=/etc/shared/document_db.pem&retryWrites=false
```

[**Find more details in this repo**](https://github.com/Portkey-AI/helm-chart/blob/main/helm/enterprise/README.md)**.**

***

## Cloud Deployment

Portkey with MongoDB integration can be deployed to all major cloud providers. For cloud-specific documentation, please refer to:

{% content-ref url="../../private-cloud-deployments/aws.md" %}
[aws.md](../../private-cloud-deployments/aws.md)
{% endcontent-ref %}

{% content-ref url="../../private-cloud-deployments/azure.md" %}
[azure.md](../../private-cloud-deployments/azure.md)
{% endcontent-ref %}

{% content-ref url="../../private-cloud-deployments/gcp.md" %}
[gcp.md](../../private-cloud-deployments/gcp.md)
{% endcontent-ref %}

[Get started with Portkey Enterprise here](../../).
