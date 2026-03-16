## AWS Route53

<br/>

### 1. Create Secret

Create a Kubernetes Secret with your AWS Secret Access Key:

```bash
kubectl create secret generic route53-credentials-secret \
  --from-literal=secret-access-key=AWS_SECRET_ACCESS_KEY \
  --namespace=cert-manager
```

<br/>

### 2. Create ClusterIssuer

The credentials secret is created in the `cert-manager` namespace. The ClusterIssuer is a cluster-scoped resource and does not belong to any namespace.

```bash
kubectl apply -f route53-credentials-secret.yaml -n cert-manager
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

- [Route53 DNS01](https://cert-manager.io/docs/configuration/acme/dns01/route53/)