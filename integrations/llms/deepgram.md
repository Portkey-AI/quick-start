# Deepgram

Portkey provides a robust and secure gateway to use and observe Deepgrm's Speech-to-Text API.

Deepgram API is currently supported on Portkey's REST API, with support for Python & Node SDKs coming soon.

## Speech to Text API

* We set the target Deepgram API URL with the `x-portkey-custom-host` header
* We set the target provider as `openai` to let Portkey know that this request should be handled similarly to OpenAI

{% tabs %}
{% tab title="Local File" %}
<pre class="language-bash"><code class="lang-bash">curl 'https://api.portkey.ai/v1/listen' \
    -H 'Authorization: Token $DEEPGRAM_API_KEY' \
    -H 'Content-Type: audio/mp3' \
<strong>    -H 'x-portkey-custom-host: https://api.deepgram.com/v1' \
</strong><strong>    -H 'x-portkey-provider: openai' \
</strong><strong>    -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
</strong>    --data-binary '@audio.mp3'
</code></pre>
{% endtab %}

{% tab title="Remote File" %}
<pre class="language-bash"><code class="lang-bash">curl 'https://api.portkey.ai/v1/listen' \
    -H 'Authorization: Token $DEEPGRAM_API_KEY' \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
<strong>    -H 'x-portkey-custom-host: https://api.deepgram.com/v1' \
</strong><strong>    -H 'x-portkey-provider: openai' \
</strong><strong>    -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
</strong>    -d '{"url": "https://dpgr.am/spacewalk.wav"}'
</code></pre>
{% endtab %}
{% endtabs %}

Refer to Deepgram API docs for more:

{% embed url="https://developers.deepgram.com/reference/listen-file" %}
