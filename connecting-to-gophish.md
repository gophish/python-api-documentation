# Connecting to Gophish

Connections to Gophish are established by creating an instance of `gophish.Gophish()`.

By default, the API client will try connecting to the host at `http://localhost:3333`.

## Changing the Host

To change the host, simply set the host parameter:

``` python
from gophish import Gophish

api = Gophish(API_KEY, host='http://admin_server')
```

## Ignoring SSL Certificates

All custom `kwargs` are sent to the underlying transport, which by default is the `requests` library.

This means it's easy to customize client behavior. For example, if you are using self-signed certificates with Gophish, you can ignore the warnings by setting `verify=False`.

``` python
from gophish import Gophish

api = Gophish(API_KEY, host='http://admin_server', verify=False)
```