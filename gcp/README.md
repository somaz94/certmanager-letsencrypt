## cloud-dns

- Create Kubernetes Secret with GCP ServiceAccount

```bash
kubectl create secret generic clouddns-credentials-secret \
--from-file=key.json=/path/to/SERVICE_ACCOUNT_KEY.json \
--namespace=[CERT-MANAGER-NAMESPACE] > clouddns-credentials-secret.yaml
```

- And sequentially, the clusterissuer, certificate, and ingress are created.
- The clouddns credentials secret is created in cert-manager namespace, and the Clusterissuer does not belong to namespace.

```bash
k apply -f clouddns-credentials-secret.yaml -n certmanager
k apply -f clusterissuer.yaml  # No namespace
```