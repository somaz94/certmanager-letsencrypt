## certmanager-letsencrypt
Describes how to install certmanager and set up each dns provider.

## certmanager install(common)
Please refer to the reference below for the latest version.

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.3/cert-manager.yaml

k get po -n cert-manager
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-75f8fbb664-q4k6f              1/1     Running   0          33s
cert-manager-cainjector-69448777d5-hnz5d   1/1     Running   0          33s
cert-manager-webhook-694b449697-7d4zp      1/1     Running   0          33s
```

## Reference
- [certmanager](https://github.com/cert-manager/cert-manager/releases)
- [certmanager-issuer-setting](https://cert-manager.io/docs/configuration/acme/dns01/)