# gitops-demo

Monorepo GitOps pour le tuto **K3s HA + GitOps** sur [yoandev.co](https://yoandev.co).

## Structure

```
.
├── bootstrap/
│   ├── root-app.yaml             # Application ArgoCD racine (app-of-apps)
│   └── applications/             # Applications enfants (générées par root-app)
│       └── cloudflared.yaml
├── infra/
│   └── cloudflared/              # Manifestes Cloudflare Tunnel connector
│       ├── namespace.yaml
│       └── deployment.yaml
└── apps/                         # Applications métier (rempli au chap. 7)
```

## Pattern app-of-apps

`root-app.yaml` watch le dossier `bootstrap/applications/` en mode `directory.recurse: true`. Tout fichier YAML qui y est ajouté devient une `Application` ArgoCD synchronisée automatiquement.

## Bootstrap

```bash
# 1. Install ArgoCD (manuel, une fois)
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 2. Apply la root-app
kubectl apply -f bootstrap/root-app.yaml
```

À partir de là, toute modification se fait via Git.
