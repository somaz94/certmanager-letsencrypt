## Route53

- Create Kubernetes Secret with AWS Secret Access Key.

```bash
# CLI Create
kubectl create secret generic route53-credentials-secret \
--from-literal=secret-access-key=YOUR_AWS_SECRET_ACCESS_KEY
--namespace=[YOUR-CERT-MANAGER-NAMESPACE]
```

- And sequentially, the clusterissuer certificate finally generates ingress.
- The route53 credentials secret is created in cert-manager namespace, and the Clusterissuer does not belong to namespace.
