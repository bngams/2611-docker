---
title: "Docker Compose"
---

Réseau, volumes, variables, ports… trop de choses à retenir et à transmettre. **Compose** décrit tout le stack dans un fichier YAML versionné à la racine du projet : un `git clone` + une commande, et c'est parti.

## Traduire les `docker run` en `compose.yaml`

À la racine de `getting-started-app`, crée `compose.yaml` :

```yaml
services:
  app:
    image: node:22-alpine
    command: sh -c "npm ci && npm run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.4
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

Correspondances à repérer :

| `docker run` | `compose.yaml` |
|---|---|
| `-p` | `ports` |
| `-w` | `working_dir` |
| `-v "$(pwd):/app"` | `volumes: - ./:/app` (chemin relatif !) |
| `-e` | `environment` |
| `--network-alias mysql` | le **nom du service** devient l'alias |
| volume créé à la volée | doit être déclaré dans `volumes:` de premier niveau |
| `docker network create` | rien : Compose crée un réseau par projet |

## Lancer le stack

```bash
docker ps                     # rien ne doit tourner
docker rm -f <ids>            # si besoin
docker compose up -d
docker compose logs -f        # ou : docker compose logs -f app
```

`✓ checkpoint` — Compose crée un réseau `getting-started-app_default`, le volume, deux conteneurs ; les logs entrelacés montrent `ready for connections` côté MySQL et `Connected to mysql db at host mysql` côté app.

Le **nom de projet** est par défaut celui du dossier ; les conteneurs sont nommés `<projet>-<service>-<n>`.

`⚠ à surveiller` — l'app peut démarrer avant que MySQL soit prêt. Elle attend alors le port 3306 pendant 10 secondes au maximum (`Waiting for mysql:3306...` dans les logs), puis s'arrête en erreur si la base n'a pas répondu. Les logs entrelacés servent justement à voir ces problèmes de timing. Le défi final règle ce point (B1).

## Tout démonter

```exec
docker compose down             # conteneurs + réseau
docker compose down --volumes   # … + volumes nommés
```

> Par défaut, `down` **ne supprime pas** les volumes. Le bouton corbeille de Docker Desktop non plus.
