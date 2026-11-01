# Split horizon DNS with trusted certs at home

Type the same `https://` address at home and away, get a trusted certificate both times, and never open a port on your router.

Watch the video: {{VIDEO_URL}}

This is **my** answer to the problem, not *the* answer. Take what is useful and change the rest.

## What's here

| Path | Video section |
|---|---|
| [`steps/01-cloudflare-tunnel/`](steps/01-cloudflare-tunnel) | Cloudflare: proxied wildcard and Tunnel (body_2, body_12) |
| [`steps/02-cert-manager-issuer/`](steps/02-cert-manager-issuer) | DNS-01 with a scoped Cloudflare token (body_5, body_10) |
| [`steps/03-ingress-certificate/`](steps/03-ingress-certificate) | Ingress annotation and explicit Certificate (body_7, body_11) |
| [`steps/04-split-horizon-dns/`](steps/04-split-horizon-dns) | AdGuard rewrites and external-dns fallback (body_6, body_9, body_13) |
| [`steps/05-verify/`](steps/05-verify) | dig / curl / kubectl checks (body_14) |
| [`steps/06-tailscale-subnet-router/`](steps/06-tailscale-subnet-router) | Remote access (body_15) |
| [`docs/rate-limits.md`](docs/rate-limits.md) | Let's Encrypt limits (body_3) |

## Prerequisites

- A domain you own, with its DNS on Cloudflare. On the free plan Cloudflare needs the root domain, so DuckDNS or deSEC subdomains will not work for this.
- A Kubernetes cluster with cert-manager (the video pins chart `1.21.3`), Traefik as Ingress controller, and optionally Argo CD.
- A Cloudflare API token limited to one zone with **Zone: DNS: Edit** and **Zone: Zone: Read**. Scope is per zone, not per record type, so it cannot be "TXT only".
- A local DNS server (the video uses AdGuard Home) or the external-dns fallback.
- Optional: a Raspberry Pi (or any Linux box outside the cluster) and a Tailscale account.

Everywhere you see `example.com`, `YOUR_*` or `192.168.1.x`, replace it with your own values.

## Follow along

1. **Cloudflare**: `steps/01-...`. Terraform for the proxied wildcard CNAME to a Cloudflare Tunnel.
2. **Issuer**: `steps/02-...`. Create the token Secret, then the ClusterIssuer. **Start on staging**, flip to production once everything works.
   ```bash
   kubectl get clusterissuer
   ```
3. **Certificates**: `steps/03-...`. Apply the Ingress, then check:
   ```bash
   kubectl get certificate
   kubectl describe certificate <name>
   ```
4. **Split DNS**: `steps/04-...`. Pick AdGuard rewrites, external-dns private records, or both.
5. **Verify**: `steps/05-...`.
6. **Remote access**: `steps/06-...`.

## Notes and caveats

- Outside your network you get Cloudflare's edge certificate. Inside you get the Let's Encrypt one served by Traefik. Both are trusted, but they are not the same certificate.
- The video's zone uses SSL mode **Full**, which does not validate the origin certificate. Full (strict) would, and should accept Let's Encrypt.
- If DNS-01 self-checks time out on a split-DNS setup, see the optional controller flags in `steps/05-verify/README.md`. The video's setup does not need them.
- These files are cleaned-up, generic versions of what the video showed, with placeholders for hostnames, IPs and tokens. They are not tested as-is.

## License

MIT
