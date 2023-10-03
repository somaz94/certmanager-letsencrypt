## cloudflare

- Create an Access Token after creating a CloudFlare account
- Create an API token after going to the "My Profile" section.
- The API token permission is created by granting zone-zone(영역-영역) and read permission and zone-dns(영역-dns) permission to edit.
- Create Kubernetes Secret with CloudFlare Token.

```bash
# CLI Create
kubectl create secret generic cloudflare-api-token-secret --namespace cert-manager \
--from-literal=api-token=YOUR_CLOUDFLARE_API_TOKEN
--namespace=[YOUR-CERT-MANAGER-NAMESPACE]
```

- And sequentially, the clusterissuer, certificate, and ingress are created.
- The api token secret is created in cert-manager namespace, and the Clusterissuer does not belong to namespace.

```bash
k apply -f cloudflare-api-token-secret.yaml -n certmanager
k apply -f clusterissuer.yaml  # No namespace
```


- Finally, create the remaining resources.
- Note that you should create the namespace of the application to be applied.

```bash
k apply -f certificate.yaml -n <application namespace>
k apply -f ingress.yaml -n <application namespace>
```

<br/>

## Reference
- [cloudflare](https://www.cloudflare.com/ko-kr/)
- [cloudflare-api-keys](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/#api-keys)