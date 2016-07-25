# Authorization / API Key

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.shareactor.io/"
  -H "Authorization: meowmeowmeow"
```

```python
import requests

headers = {'Authorization': 'Bearer <shareactor-api-key>'}

requests.get('https://api.shareactor.io/...', headers=headers)
```

> Make sure to replace `<shareactor-api-key>` with your API key.

Shareactor uses API keys to allow access to the API. If you want access to our APIs please get in touch on our [website](http://www.shareactor.io).

All API requests should include this key in the headers like the following:

`Authorization: Bearer <shareactor-api-key>`

<aside class="notice">
You must replace <code>shareactor-key</code> with your personal API key.
</aside>
