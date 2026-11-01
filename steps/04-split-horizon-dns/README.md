# Step 4: Same name, different answer

Outside, the name leads to Cloudflare. Inside, it leads to a machine on your LAN. Pick one (or both).

## Option A: AdGuard Home DNS rewrites (the "proper" way)

`adguard-rewrites.example.yaml` has the `rewrites` section of `AdGuardHome.yaml`, wildcard included. Then make sure devices use it: hand out the DNS server through your router's DHCP settings, and hardcode it on devices that ignore DHCP. Works the same with Pi-hole, NextDNS or whatever you run.

## Option B: Cloudflare record with a private IP

`external-dns.example.yaml` publishes only the Ingresses annotated `ingress.type=private` as **DNS-only** records holding your node LAN IPs. An exact record beats the proxied wildcard from step 1. In the video this is the fallback for devices that ignore the local DNS server. Anyone outside who looks the name up gets an address that means nothing on their network.

The same API token feeds external-dns, so it needs DNS edit anyway.

Checkpoint: a device on your LAN gets a LAN address for your names.
