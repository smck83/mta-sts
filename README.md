# mta-sts

Hosts the [MTA-STS](https://datatracker.ietf.org/doc/html/rfc8461) policy for `poscha.com` via GitHub Pages.

Policy URL: <https://mta-sts.poscha.com/.well-known/mta-sts.txt>

## Files

| File | Purpose |
| --- | --- |
| `.well-known/mta-sts.txt` | The policy itself |
| `CNAME` | Custom domain for GitHub Pages |
| `.nojekyll` | Stops Jekyll skipping the `.well-known` dot-directory |

## DNS records (poscha.com)

| Name | Type | Value |
| --- | --- | --- |
| `mta-sts.poscha.com` | CNAME | `smck83.github.io` |
| `_mta-sts.poscha.com` | TXT | `v=STSv1; id=20261023000000;` |
| `poscha.com` | MX | `10 proxy.ehlo.email` (must match the `mx:` line in the policy) |

Optional, to receive TLS failure reports:

| Name | Type | Value |
| --- | --- | --- |
| `_smtp._tls.poscha.com` | TXT | `v=TLSRPTv1; rua=mailto:tlsrpt@poscha.com` |

## GitHub Pages setup

Settings → Pages → Deploy from branch `main`, folder `/ (root)`. Custom domain `mta-sts.poscha.com`, and tick **Enforce HTTPS** once the certificate is issued (MTA-STS requires a valid HTTPS certificate).

## Changing the policy

1. Edit `.well-known/mta-sts.txt` (e.g. `mode: testing` → `mode: enforce`).
2. Bump the `id=` in the `_mta-sts` TXT record (any new value; a timestamp works) so senders refetch the policy.

## Other domains

GitHub Pages allows only one custom domain per repository. Each extra domain needs its own repo (with its own `CNAME`), or move hosting to something that supports multiple hostnames, such as a Cloudflare Worker or Vercel.
