# DevOps – GitOps / Kustomize / Argo CD

Ce dépôt contient une petite démo GitOps basée sur :

- **Kubernetes + k3s**
- **Kustomize** pour gérer les environnements
- **Argo CD** pour déployer automatiquement depuis ce repo

L’objectif : montrer comment passer d’un **manifeste Kubernetes de base** à plusieurs environnements (dev / prod) sans dupliquer tout le YAML.

---

## Structure du dépôt

```
.
├── base/
├── overlays/
│   ├── dev/
│   └── prod/
└── app/
```

### `base/` – Manifests communs

Ce dossier contient les **ressources Kubernetes génériques**, communes à tous les environnements, par exemple :

- `Deployment`
- `Service`
- `ConfigMap` (page HTML NGINX, etc.)

Ici, il n’y a **aucune notion d’environnement** : pas de `dev`, pas de `prod`.  
C’est la “vérité de base” sur laquelle les overlays viennent se greffer.

---

### `overlays/` – Environnements (Kustomize)

Ce dossier contient les overlays **Kustomize** pour chaque environnement :

- `overlays/dev/`
- `overlays/prod/`

Chaque overlay :

- référence `../../base`
- définit son **namespace** (ex : `dev`, `prod`)
- applique des **patches** sur les ressources de `base` (par ex. couleur de fond de la page HTML, nombre de replicas, etc.)

Exemples de variations possibles :

- couleur du fond dans `index.html`
- nombre de pods (`replicas`)
- image/tag Docker
- annotations / labels spécifiques

---

### `app/` – Applications Argo CD


Avec Argo CD, les fichiers du dossier `app/` sont utilisés pour créer les Applications qui suivront automatiquement les changements de ce dépôt.
