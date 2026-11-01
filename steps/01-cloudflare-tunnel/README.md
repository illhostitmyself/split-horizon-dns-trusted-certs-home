# Step 1: Cloudflare proxied wildcard and Tunnel

One proxied wildcard CNAME points everything under your domain at a Cloudflare Tunnel. The tunnel has a single rule sending all traffic to Traefik. `cloudflared` only makes outbound connections, so no router ports are opened.

Needs: Terraform >= 1.6, Cloudflare provider v5, a token that can edit the zone and Tunnels. Set `cloudflare_account_id`, `cloudflare_zone_id` and `domain`, and export the token as `CLOUDFLARE_API_TOKEN` (never commit it).

Zone SSL mode is `full`, as in the video.

A wildcard only applies where there is no exact record, so the DNS-only records in step 4 win over this one.
