# certmanager-letsencrypt Guide

[![License](https://img.shields.io/github/license/somaz94/certmanager-letsencrypt)](LICENSE)
[![Lint](https://github.com/somaz94/certmanager-letsencrypt/actions/workflows/lint.yml/badge.svg)](https://github.com/somaz94/certmanager-letsencrypt/actions/workflows/lint.yml)
![GitHub Stars](https://img.shields.io/github/stars/somaz94/certmanager-letsencrypt?style=social)

This guide provides steps for installing cert-manager and setting up various DNS providers with Let's Encrypt using DNS-01 challenge.

> **Helm chart users**: the chart previously in `helm/` has moved to **[`somaz94/helm-charts`](https://github.com/somaz94/helm-charts/tree/main/charts/certmanager-letsencrypt)** with multi-issuer / multi-certificate / multi-secret support. Install via `oci://ghcr.io/somaz94/charts/certmanager-letsencrypt` or `https://charts.somaz.blog`. The local `helm/` directory remains for reference only — see [`helm/README.md`](helm/README.md).

<br/>

## Features

![AWS Route53](https://img.shields.io/badge/AWS_Route53-FF9900?logo=amazonaws&logoColor=white)
![Google Cloud DNS](https://img.shields.io/badge/Google_Cloud_DNS-4285F4?logo=googlecloud&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white)
![Helm](https://img.shields.io/badge/Helm_Chart-0F1689?logo=helm&logoColor=white)
![Let's Encrypt](https://img.shields.io/badge/Let's_Encrypt-003A70?logo=letsencrypt&logoColor=white)

- DNS-01 challenge support for wildcard certificates
- Support for multiple DNS providers: AWS Route53, Google Cloud DNS, Cloudflare
- Plain YAML manifests for hands-on / GitOps workflows
- 90-day certificate lifecycle with automatic 30-day renewal
- ClusterIssuer with ACME Let's Encrypt production endpoint
- Reusable Helm chart published separately at [`somaz94/helm-charts`](https://github.com/somaz94/helm-charts/tree/main/charts/certmanager-letsencrypt)

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
└── helm/                   # Legacy Helm chart (deprecated — see helm/README.md)
    ├── values-aws.yaml     # provider example values still useful as DNS-01 schema reference
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

> **The chart in `helm/` is deprecated.** Use the centralized version at [`somaz94/helm-charts`](https://github.com/somaz94/helm-charts/tree/main/charts/certmanager-letsencrypt) — published to `https://charts.somaz.blog` (classic Helm repo) and `oci://ghcr.io/somaz94/charts/certmanager-letsencrypt` (OCI registry). The new chart adds multi-issuer / multi-certificate / multi-secret support and a `values.schema.json`.

**Install (new location)**

```bash
# Recommended: OCI registry (Helm 3.8+)
helm install cert oci://ghcr.io/somaz94/charts/certmanager-letsencrypt \
  --version 0.1.0 \
  --namespace cert-manager \
  -f my-values.yaml

# Alternative: classic Helm repo
helm repo add somaz94 https://charts.somaz.blog
helm install cert somaz94/certmanager-letsencrypt \
  --version 0.1.0 \
  --namespace cert-manager \
  -f my-values.yaml
```

The new chart is **not a drop-in replacement** — `values.yaml` shape differs. See the new chart's [README](https://github.com/somaz94/helm-charts/blob/main/charts/certmanager-letsencrypt/README.md) and [`helm/README.md`](helm/README.md) for the schema differences and migration notes.

The legacy `helm/` directory in this repo remains as a reference (its provider-specific `values-*.yaml` files still illustrate DNS-01 solver shapes per provider) but receives no further updates.

<br/>

## Reference

- [cert-manager Releases](https://github.com/cert-manager/cert-manager/releases)
- [ACME DNS-01 Configuration](https://cert-manager.io/docs/configuration/acme/dns01/)

<br/>

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
