# Step 6: Reach private apps away from home

A Tailscale subnet router on a Raspberry Pi **outside the cluster**, so remote access survives a cluster outage. Chosen over plain WireGuard to avoid router port forwards and dynamic DNS, and it works behind CGNAT. The trade-off is relying on Tailscale's coordination service.

On the Pi (replace the subnet with your LAN):

```bash
echo 'net.ipv4.ip_forward = 1' | sudo tee /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
sudo tailscale up --advertise-routes=192.168.1.0/24 --accept-dns=false
```

No exit node. Then:

1. Approve the route in the Tailscale admin console (Machines).
2. Linux clients: `sudo tailscale up --accept-routes`. Other clients: enable "Use Tailscale subnets".
3. On the admin console DNS page, add split DNS for your internal domain pointing at your own resolver.
