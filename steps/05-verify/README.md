# Step 5: Test from both sides

From inside the LAN:

```bash
dig app.example.com            # should return the LAN IP
curl -v https://app.example.com 2>&1 | grep -Ei "issuer|verify"
```

Look for Let's Encrypt as the issuer and `SSL certificate verify ok`.

From outside (mobile data), for a name you made public:

```bash
dig app.example.com            # should return Cloudflare addresses
```

## If a certificate gets stuck on a split-DNS setup

Before asking Let's Encrypt, cert-manager checks the TXT record itself, and with split DNS that check can time out. The docs give two controller flags. Set them through the Helm extra args (`cert-manager-values.example.yaml`). The video's setup uses neither, so only add them if you need to.
