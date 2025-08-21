---
title: "Unsafe legacy renegotiation disable error in OpenSSL"
date: 2023-08-13
tags:
  - networking
  - programming
categories:
  - networking
author_profile: true
draft: false
comments: false
---

The new OpenSSL 3 is a major change from the old OpenSSL 1.1.1. A complete list of all changes and a migration guide can be found at [this link](https://www.openssl.org/docs/manmaster/man7/migration_guide.html).

One important change is that it is now required by default to use a secure renotiation protocol for SSL or TSL to succeed, as established by RFC 5746. It is still possible to connect to legacy servers using `SSL_OP_LEGACY_SERVER_CONNECT` that is not part of `SSL_OP_ALL`.

An outdated endpoint that does not support RFC 5746 secure renegotiation will return and error. On Python this may look similar to

```bash
(Caused by SSLError(SSLError(1, '[SSL: UNSAFE_LEGACY_RENEGOTIATION_DISABLED] unsafe legacy renegotiation disabled (_ssl.c:1007)')))
```

## Python solution

Combinig two GitHub solutions ([here](https://github.com/psf/requests/issues/4775) the first, [here](https://github.com/python/cpython/pull/27776) the second), the following approach worked for me in connecting to legacy systems.

```python
import requests
from requests import adapters
import ssl
from urllib3 import poolmanager


class TLSAdapter(adapters.HTTPAdapter):

    def init_poolmanager(self, connections, maxsize, block=False):
        """Create and initialize the urllib3 PoolManager."""
        ctx = ssl.create_default_context(ssl.Purpose.SERVER_AUTH)
        ctx.options |= 0x4
        ctx.set_ciphers('DEFAULT@SECLEVEL=1')
        self.poolmanager = poolmanager.PoolManager(
                num_pools=connections,
                maxsize=maxsize,
                block=block,
                ssl_version=ssl.PROTOCOL_TLS,
                ssl_context=ctx
        )

session = requests.session()
session.mount('https://', TLSAdapter())
session.get(TARGET)
```

where the line

```python
ctx.options |= 0x4
```

is used to enable `SSL_OP_LEGACY_SERVER_CONNECT`.

## System-wide solution

To enable the unsecure legacy connection option system-wide, change the file `/etc/ssl/openssl.cnf` making sure to have the following lines

```toml
[ openssl_init ]
ssl_conf = ssl_sect

[ ssl_sect ]
system_default = system_default_sect

[ system_default_sect ]
Options = UnsafeLegacyRenegotiation
```

Note that this lowers the overall security of the system and should only be used as a temporary measure.
