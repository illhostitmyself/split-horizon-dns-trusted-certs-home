# Step 3: Ask for a certificate

Two ways, both end with a Secret that Traefik serves.

- **Ingress annotation** (`ingress.example.yaml`): cert-manager's ingress-shim creates the Certificate in the Ingress's namespace and stores the result in `tls[].secretName`. This is what almost every app in the video uses.
- **Explicit Certificate** (`certificate.example.yaml`): for resources like IngressRoutes that cert-manager does not handle natively.

```bash
kubectl apply -f ingress.example.yaml
kubectl get certificate          # wait for READY True
kubectl describe certificate app-tls   # shows the renewal time
kubectl get secret app-tls
```

Default duration is 90 days and cert-manager renews at two thirds, around day 60. If a certificate stays not ready, run `describe` and follow the events (Certificate, CertificateRequest, Order, Challenge).

With the staging issuer browsers will warn. That is expected. Switch the annotation to the production issuer once it works.
