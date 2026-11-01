# Step 2: cert-manager and a DNS-01 ClusterIssuer

1. Create the token Secret (file `secret.example.yaml`). Prefer creating it with `kubectl create secret` so the token never lands in git:
   ```bash
   kubectl create secret generic cloudflare-api-token \
     --namespace cert-manager \
     --from-literal=api-token='YOUR_CLOUDFLARE_API_TOKEN'
   ```
2. Install cert-manager (chart `1.21.3` in the video). `argocd-application.example.yaml` shows how it is pinned with Argo CD.
3. Apply a ClusterIssuer. Start with `clusterissuer-staging.yaml`, then use `clusterissuer-production.yaml`.
   ```bash
   kubectl apply -f clusterissuer-staging.yaml
   kubectl get clusterissuer
   ```
   It should show `READY True`.

Token permissions: **Zone: DNS: Edit** and **Zone: Zone: Read**, limited to one zone. If Zone Read is missing, cert-manager's error names it. If you want even less trust, delegate `_acme-challenge` with a CNAME to a less privileged zone and set `cnameStrategy: Follow` on the solver (not the default).

No HTTP-01 solver and no inbound port is involved.
