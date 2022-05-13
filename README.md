# Description

Warning. This is archived role extracted from infra-misc. Project is obsolete and role is kept as an archive.

This role configures an Nginx proxy to for redirecting requests to `ipfs.infura.io` via a wildcard domain in order to silo cookies.

# Details

The core of the work is done by the [`files/b32tob38.lua`](files/b32tob38.lua) which is used by Nginx in order to convert the base32 hash in the domain into the base58 hash for the Infura URL.

For example:
* https://ciqmvj2c3jypqboylu5uhxd2yg7fsggqvwhinhedfhdfxh3pzuuhfea.infura.status.im - This turns into:
* https://ipfs.infura.io/ipfs/QmbyizSHLirDfZhms75tdrrdiVkaxKvbcLpXzjB5k34a31 - This.

Since IPFS addresses are static length we shouldn't see issues with padding.

The ceritificate is created using [Let'sEncrypt](https://letsencrypt.org/) and [CertBot](https://certbot.eff.org/) ran with a System timer.

# Configuration

Only mandatory settings are:
```yaml
certbot_cloudflare_api_token: 'this-is-your-secret-api-token'
certbot_domain: *.inura.example.org
```
Take not that the token has to be one with restricted access.

# Links

* https://github.com/ricmoo/meeseeks-app
* https://certbot-dns-cloudflare.readthedocs.io/en/stable/

# Known Issues

* If you specify a `resolver` config for Nginx it will break this proxy due to Nginx trying to use IPv6:
  - `connect() to [IPv6addr]:443 failed (101: Network is unreachable) while connecting to upstream`
