# Helm chart (deprecated — moved to somaz94/helm-charts)

> **This chart has been moved.** Please use the centrally-maintained version at:
>
> **https://github.com/somaz94/helm-charts/tree/main/charts/certmanager-letsencrypt**

The new location is published to:
- **OCI registry** (recommended): `oci://ghcr.io/somaz94/charts/certmanager-letsencrypt`
- **Classic Helm repo**: `https://charts.somaz.blog`

## Install (new location)

```bash
# OCI (Helm 3.8+)
helm install cert oci://ghcr.io/somaz94/charts/certmanager-letsencrypt --version 0.1.0

# Classic Helm repo
helm repo add somaz94 https://charts.somaz.blog
helm install cert somaz94/certmanager-letsencrypt --version 0.1.0
```

## What changed

The relocated chart is a redesign with the same intent (Let's Encrypt + cert-manager wrapper) but a different schema. **It is not a drop-in replacement** for the legacy chart in this directory — `values.yaml` shape differs. If migrating an existing release, expect to rewrite values.

| Aspect | This chart (legacy, frozen) | New chart (`somaz94/helm-charts`) |
|---|---|---|
| Multi-issuer support | ❌ (single ClusterIssuer) | ✅ (`clusterIssuers[]` + `issuers[]`) |
| Multi-certificate support | ❌ (single Certificate) | ✅ (`certificates[]`) |
| Multi-secret support | ❌ (single Secret) | ✅ (`secrets[]`) |
| Ingress template | ✅ (creates one) | ❌ (intentional — apps own their Ingress) |
| `values.schema.json` | ❌ | ✅ (JSON Schema draft-07 validation) |
| ACME server presets | ❌ | ✅ (`acmeServers.letsencryptProd` / `letsencryptStaging`) |
| ArtifactHub catalog | ❌ | ✅ (auto-indexed) |

The provider example values files (`values-aws.yaml`, `values-cloudflare.yaml`, `values-gcp.yaml`) in this directory remain useful as shape references — the new chart's README has equivalent per-provider examples.

## Why deprecate this directory instead of deleting?

- The parent repo (`certmanager-letsencrypt/`) still has useful raw-manifest example directories (`aws/`, `gcp/`, `cloudflare/`) that document the bare-metal setup.
- Existing users referencing `helm/` paths get an immediate signal here without a 404.
- A future cleanup PR may remove this `helm/` directory entirely once it's clearly no longer in use.
