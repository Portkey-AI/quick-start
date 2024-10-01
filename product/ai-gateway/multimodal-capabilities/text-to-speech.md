# Text-to-Speech

Portkey's AI gateway currently supports text-to-speech models on `OpenAI` and `Azure OpenAI`.

### Usage

We follow the OpenAI signature where you can send the input text and the voice option as a part of the API request. All the output formats `mp3`, `opus`, `aac`, `flac`, and `pcm` are supported. Portkey also supports real time audio streaming for TTS models.

Here's an example:

{% tabs %}
{% tab title="OpenAI NodeJS" %}
```javascript
import fs from "fs";
import path from "path";
import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "OPENAI_VIRTUAL_KEY"
  })
});

const speechFile = path.resolve("./speech.mp3");

async function main() {
  const mp3 = await openai.audio.speech.create({
    model: "tts-1",
    voice: "alloy",
    input: "Today is a wonderful day to build something people love!",
  });
  const buffer = Buffer.from(await mp3.arrayBuffer());
  await fs.promises.writeFile(speechFile, buffer);
}

main();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from pathlib import Path
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        virtual_key="OPENAI_VIRTUAL_KEY"
    )
)

speech_file_path = Path(__file__).parent / "speech.mp3"

response = client.audio.speech.create(
  model="tts-1",
  voice="alloy",
  input="Today is a wonderful day to build something people love!"
)

f = open(speech_file_path, "wb")
f.write(response.content)
f.close()
```
{% endtab %}

{% tab title="REST" %}
```bash
curl "https://api.portkey.ai/v1/audio/speech" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "tts-1",
    "input": "Today is a wonderful day to build something people love!",
    "voice": "alloy"
  }' \
  --output speech.mp3
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from pathlib import Path
from portkey_ai import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Add your provider's virtual key
)

speech_file_path = Path(__file__).parent / "speech.mp3"

response = portkey.audio.speech.create(
  model="tts-1",
  voice="alloy",
  input="Today is a wonderful day to build something people love!"
)

f = open(speech_file_path, "wb")
f.write(response.content)
f.close()
```
{% endtab %}
{% endtabs %}

On completion, the request will get logged in the logs UI and show the cost and latency incurred.

### Supported Providers and Models

The following providers are supported for text-to-speech with more providers getting added soon. Please raise a [request](../../../integrations/llms/suggest-a-new-integration.md) or a [PR](https://github.com/Portkey-AI/gateway/pulls) to add model or provider to the AI gateway.

<table><thead><tr><th width="272.3333333333333">Provider</th><th>Models</th></tr></thead><tbody><tr><td><a href="../../../integrations/llms/openai/">OpenAI</a></td><td><code>tts-1</code><br><code>tts-1-hd</code></td></tr><tr><td>Azure OpenAI</td><td><code>tts-1</code><br><code>tts-1-hd</code></td></tr><tr><td>Deepgram (Coming Soon)</td><td></td></tr><tr><td>ElevanLabs (Coming Soon)</td><td></td></tr></tbody></table>
