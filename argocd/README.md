# ArgoCD Application Manifests

This directory contains ArgoCD Application manifests for deploying Kartograph to various environments.

## Structure

```
argocd/
├── kartograph-stage.yaml    # Stage environment
└── kartograph-prod.yaml     # Production environment (future)
```

## Applying Applications

### Stage Environment

```bash
kubectl apply -f argocd/kartograph-stage.yaml
```

This creates an ArgoCD Application that:

- Points to `apps/kartograph/overlays/stage` in this repository
- Deploys to the `kartograph-stage` namespace
- Auto-syncs on git changes (automated deployment)
- Uses Secrets Store CSI Driver with Vault for secrets

## How It Works

1. **Code changes** → Push to `main` branch
2. **Release workflow** → Bumps version, updates image tag in kustomization.yaml
3. **ArgoCD** → Detects change, syncs new image to cluster
4. **Vault CSI Driver** → Mounts secrets from Vault at pod startup

## Configuration

- **Repository**: `https://github.com/jsell-rh/kartograph.git`
- **Target Revision**: `main` (tracks main branch)
- **Sync Policy**: Automated with prune and self-heal enabled
