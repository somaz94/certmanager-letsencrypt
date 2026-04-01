# certmanager-letsencrypt Guide

[![License](https://img.shields.io/github/license/somaz94/certmanager-letsencrypt)](LICENSE)
[![Lint](https://github.com/somaz94/certmanager-letsencrypt/actions/workflows/lint.yml/badge.svg)](https://github.com/somaz94/certmanager-letsencrypt/actions/workflows/lint.yml)
![GitHub Stars](https://img.shields.io/github/stars/somaz94/certmanager-letsencrypt?style=social)

This guide provides steps for installing cert-manager and setting up various DNS providers with Let's Encrypt using DNS-01 challenge.

<br/>

## Features

![AWS Route53](https://img.shields.io/badge/AWS_Route53-FF9900?logo=amazonaws&logoColor=white)
![Google Cloud DNS](https://img.shields.io/badge/Google_Cloud_DNS-4285F4?logo=googlecloud&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white)
![Helm](https://img.shields.io/badge/Helm_Chart-0F1689?logo=helm&logoColor=white)
![Let's Encrypt](https://img.shields.io/badge/Let's_Encrypt-003A70?logo=letsencrypt&logoColor=white)

- DNS-01 challenge support for wildcard certificates
- Support for multiple DNS providers: AWS Route53, Google Cloud DNS, Cloudflare
- Plain YAML manifests and Helm chart deployment options
- 90-day certificate lifecycle with automatic 30-day renewal
- ClusterIssuer with ACME Let's Encrypt production endpoint

<br/>

## Supported DNS Providers

| Provider | Directory | Documentation |
|----------|-----------|---------------|
| AWS Route53 | [aws/](aws/) | [Route53 DNS01](https://cert-manager.io/docs/configuration/acme/dns01/route53/) |
| Google Cloud DNS | [gcp/](gcp/) | [Google CloudDNS](https://cert-manager.io/docs/configuration/acme/dns01/google/) |
| Cloudflare | [cloudflare/](cloudflare/) | [Cloudflare DNS01](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/) |

<br/>

## Repository Structure

```
certmanager-letsencrypt/
├── aws/                    # AWS Route53 manifests
│   ├── route53-credentials-secret.yaml
│   ├── clusterissuer.yaml
│   ├── certificate.yaml
│   └── ingress.yaml
├── gcp/                    # Google Cloud DNS manifests
│   ├── clouddns-credentials-secret.yaml
│   ├── clusterissuer.yaml
│   ├── certificate.yaml
│   └── ingress.yaml
├── cloudflare/             # Cloudflare manifests
│   ├── cloudflare-api-token-secret.yaml
│   ├── clusterissuer.yaml
│   ├── certificate.yaml
│   └── ingress.yaml
└── helm/                   # Helm chart with provider-specific values
    ├── values-aws.yaml
    ├── values-gcp.yaml
    └── values-cloudflare.yaml
```

<br/>

## Installing cert-manager

To fetch the latest version, consult the [official cert-manager releases](https://github.com/cert-manager/cert-manager/releases).

```bash
# Using latest version
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml

# Or pin to a specific version (recommended for production)
# kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.17.2/cert-manager.yaml
```

Verify installation:
```bash
kubectl get po -n cert-manager
```

<br/>

## Usage

<br/>

### Plain YAML

Each provider directory contains the following template files:

| File | Description |
|------|-------------|
| `*-secret.yaml` | Credentials secret for DNS provider |
| `clusterissuer.yaml` | ClusterIssuer with ACME DNS-01 solver |
| `certificate.yaml` | Certificate resource (90d duration, 30d renewal) |
| `ingress.yaml` | Ingress with TLS configuration |

Refer to each provider's README for detailed setup instructions.

<br/>

### Helm Chart

The `helm/` directory provides a reusable Helm chart with provider-specific values files.

```bash
# AWS Route53
helm install cert-manager-cert ./helm -f ./helm/values-aws.yaml

# Google Cloud DNS
helm install cert-manager-cert ./helm -f ./helm/values-gcp.yaml

# Cloudflare
helm install cert-manager-cert ./helm -f ./helm/values-cloudflare.yaml
```

You can also override values inline:

```bash
helm install cert-manager-cert ./helm -f ./helm/values-aws.yaml \
  --set certificate.commonName=example.com \
  --set clusterIssuer.email=admin@example.com
```

<br/>

## Reference

- [cert-manager Releases](https://github.com/cert-manager/cert-manager/releases)
- [ACME DNS-01 Configuration](https://cert-manager.io/docs/configuration/acme/dns01/)

<br/>

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
