## cloudflare
Create an Access Token after creating a CloudFlare account
- [cloudflare](https://www.cloudflare.com/ko-kr/)

Create an API token after going to the "My Profile" section.
The API token permission is created by granting zone-zone(영역-영역) and read permission and zone-dns(영역-dns) permission to edit.
- [cloudflare-api-keys](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/#api-keys)

Create Kubernetes Secret with CloudFlare Token.
```bash
# CLI Create
kubectl create secret generic cloudflare-api-token-secret --namespace cert-manager \
--from-literal=api-token=YOUR_CLOUDFLARE_API_TOKEN
--namespace=[YOUR-CERT-MANAGER-NAMESPACE]
```

And sequentially, the clusterissuer certificate finally generates ingress.
The api token secret is created in cert-manager namespace, and the Clusterissuer does not belong to namespace.
The remaining certificates and ingress are created in the application namespace.