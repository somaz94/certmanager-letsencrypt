# certmanager-letsencrypt Guide 🚀
This guide provides steps for installing certmanager and setting up various DNS providers with Let's Encrypt.

<br/>

## 📥 Installing certmanager

### 1. Download and Install

To fetch the latest version, consult the [official certmanager releases](https://github.com/cert-manager/cert-manager/releases) releases. For version v1.12.3, run the command:
```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.3/cert-manager.yaml
```

<br/>

### 2. Verify Installation

Confirm that the certmanager components are running:
```bash
kubectl get po -n cert-manager
```

You should see:
```bash
k get po -n cert-manager
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-75f8fbb664-q4k6f              1/1     Running   0          33s
cert-manager-cainjector-69448777d5-hnz5d   1/1     Running   0          33s
cert-manager-webhook-694b449697-7d4zp      1/1     Running   0          33s
```

<br/>

## 🐵 DNS Provider Configuration
Please refer to the [official certmanager documentation](https://cert-manager.io/docs/configuration/acme/dns01/) for details on how to configure ACME DNS-01 challenge providers.

<br/>

## 🌐 Reference
- [certmanager](https://github.com/cert-manager/cert-manager/releases)
- [certmanager-issuer-setting](https://cert-manager.io/docs/configuration/acme/dns01/)

<br/>

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.