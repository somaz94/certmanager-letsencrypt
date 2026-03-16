## Google Cloud DNS

<br/>

### 1. Create Secret

Create a Kubernetes Secret with your GCP Service Account key:

```bash
kubectl create secret generic clouddns-credentials-secret \
  --from-file=key.json=/path/to/SERVICE_ACCOUNT_KEY.json \
  --namespace=cert-manager
```

<br/>

### 2. Create ClusterIssuer

The credentials secret is created in the `cert-manager` namespace. The ClusterIssuer is a cluster-scoped resource and does not belong to any namespace.

```bash
kubectl apply -f clouddns-credentials-secret.yaml -n cert-manager
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

- [Google CloudDNS](https://cert-manager.io/docs/configuration/acme/dns01/google/)