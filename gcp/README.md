## cloud-dns

- Create Kubernetes Secret with GCP ServiceAccount

```bash
kubectl create secret generic clouddns-credentials-secret \
--from-file=key.json=/path/to/SERVICE_ACCOUNT_KEY.json \
--namespace=[YOUR-CERT-MANAGER-NAMESPACE]
```

- And sequentially, the clusterissuer certificate finally generates ingress.
- The clouddns credentials secret is created in cert-manager namespace, and the Clusterissuer does not belong to namespace.