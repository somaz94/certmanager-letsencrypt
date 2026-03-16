# certmanager-letsencrypt Guide

[![License](https://img.shields.io/github/license/somaz94/certmanager-letsencrypt)](LICENSE)

This guide provides steps for installing cert-manager and setting up various DNS providers with Let's Encrypt.

<br/>

## Supported DNS Providers

| Provider | Directory | Documentation |
|----------|-----------|---------------|
| AWS Route53 | [aws/](aws/) | [Route53 DNS01](https://cert-manager.io/docs/configuration/acme/dns01/route53/) |
| Google Cloud DNS | [gcp/](gcp/) | [Google CloudDNS](https://cert-manager.io/docs/configuration/acme/dns01/google/) |
| Cloudflare | [cloudflare/](cloudflare/) | [Cloudflare DNS01](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/) |

<br/>

## Installing cert-manager

To fetch the latest version, consult the [official cert-manager releases](https://github.com/cert-manager/cert-manager/releases).

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml
```

Verify installation:
```bash
kubectl get po -n cert-manager
```

<br/>

## Usage

Each provider directory contains the following template files:

| File | Description |
|------|-------------|
| `*-secret.yaml` | Credentials secret for DNS provider |
| `clusterissuer.yaml` | ClusterIssuer with ACME DNS-01 solver |
| `certificate.yaml` | Certificate resource (90d duration, 30d renewal) |
| `ingress.yaml` | Ingress with TLS configuration |

Refer to each provider's README for detailed setup instructions.

<br/>

## Reference

- [cert-manager Releases](https://github.com/cert-manager/cert-manager/releases)
- [ACME DNS-01 Configuration](https://cert-manager.io/docs/configuration/acme/dns01/)

<br/>

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
