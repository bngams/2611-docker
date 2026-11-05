---
title: "Multi-conteneurs et réseau"
---

On remplace SQLite par MySQL. Dans le même conteneur que l'app ? **Non** — un conteneur, une responsabilité :
- l'API et la base ne se mettent pas à l'échelle de la même façon ;
- on versionne et met à jour chaque brique indépendamment ;
- en prod, la base sera peut-être un service managé ;
- plusieurs processus dans un conteneur imposent un gestionnaire de processus.

Par défaut les conteneurs s'ignorent. Pour qu'ils se parlent : **les mettre sur le même réseau**.

## Démarrer MySQL

```exec
docker network create todo-app

docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.4
```

Remarque : `todo-mysql-data` n'a jamais été créé — `docker run` crée le volume nommé automatiquement.

```bash
docker exec -it <mysql-container-id> mysql -u root -p   # mot de passe : secret
```

```sql
SHOW DATABASES;
exit
```

`✓ checkpoint` — la base `todos` apparaît dans la liste.

## Comprendre la résolution de noms

```exec
docker run -it --network todo-app nicolaka/netshoot
dig mysql
```

`✓ checkpoint` — la section `ANSWER` contient un enregistrement `A` pour `mysql` (une IP en `172.x.x.x`), servi par le DNS embarqué de Docker (`127.0.0.11`).

`mysql` n'est pas un vrai nom d'hôte : c'est l'alias réseau que tu as déclaré. L'app n'a donc qu'à se connecter à l'hôte `mysql`.

## Brancher l'app sur MySQL

L'app lit `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`.

```exec
docker run -dp 127.0.0.1:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:22-alpine \
  sh -c "npm ci && npm run dev"
```

Les logs doivent afficher `Connected to mysql db at host mysql`. Ajoute des tâches, puis :

```bash
docker exec -it <mysql-container-id> mysql -p todos
```

```sql
SELECT * FROM todo_items;
```

`✓ checkpoint` — tes tâches sont en base.

> **Secrets** — Passer un mot de passe en variable d'environnement est toléré en dev, déconseillé en prod (visible via `docker inspect`, dans les logs, les dumps…). Préfère les secrets de l'orchestrateur, montés comme fichiers. Beaucoup d'images (dont MySQL et cette app) acceptent la variante `_FILE` : `MYSQL_PASSWORD_FILE=/run/secrets/db_password`. Docker ne fait rien de magique ici, c'est l'app qui lit le fichier.

`? Q6` — Sans `--network todo-app` sur le conteneur app, quelle erreur attends-tu dans les logs, et pourquoi ?
