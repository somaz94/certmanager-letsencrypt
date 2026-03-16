## Cloudflare

<br/>

### Prerequisites

1. Create a [Cloudflare](https://www.cloudflare.com/) account
2. Go to **My Profile > API Tokens** and create an API token with the following permissions:
   - Zone - Zone: Read
   - Zone - DNS: Edit

<br/>

### 1. Create Secret

Create a Kubernetes Secret with your Cloudflare API token:

```bash
kubectl create secret generic cloudflare-api-token-secret \
  --from-literal=api-token=YOUR_CLOUDFLARE_API_TOKEN \
  --namespace=cert-manager
```

<br/>

### 2. Create ClusterIssuer

The API token secret is created in the `cert-manager` namespace. The ClusterIssuer is a cluster-scoped resource and does not belong to any namespace.

```bash
kubectl apply -f cloudflare-api-token-secret.yaml -n cert-manager
kubectl apply -f clusterissuer.yaml
```

<br/>

### 3. Create Certificate and Ingress

Create the remaining resources in your application namespace:

```bash
kubectl apply -f certificate.yaml -n <application-namespace>
kubectl apply -f ingress.yaml -n <application-namespace>
```

<br/>

## Reference

- [Cloudflare](https://www.cloudflare.com/)
- [Cloudflare DNS01](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/)