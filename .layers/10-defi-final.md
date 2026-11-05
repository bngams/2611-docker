---
title: "Défi final"
---

Pars de ton `compose.yaml` et produis une version « presque prod » :

1. **B1 — Ordre de démarrage** : ajoute un `healthcheck` au service `mysql` et un `depends_on` avec `condition: service_healthy` sur `app`.
2. **B2 — Secrets** : remplace `MYSQL_PASSWORD` / `MYSQL_ROOT_PASSWORD` par des secrets Compose et les variantes `_FILE`. Aucun mot de passe en clair dans `compose.yaml`.
3. **B3 — Image de prod** : le service `app` utilise désormais une image construite (`build: .`) avec le Dockerfile optimisé, sans bind mount ni nodemon.
4. **B4 — Exposition** : justifie ton choix `127.0.0.1` vs `0.0.0.0`.

Critères de validation :
- `docker compose down --volumes && docker compose up -d` fonctionne du premier coup, sans erreur de connexion ;
- `docker compose config` n'affiche aucun mot de passe ;
- `docker image history` montre la couche des dépendances en cache après une modif de code.
