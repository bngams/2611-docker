---
title: "Persister les données"
---

Tes tâches disparaissent à chaque nouveau conteneur. Pourquoi ?

## Expérience : la couche inscriptible

```exec
docker run --rm alpine touch greeting.txt
docker run --rm alpine stat greeting.txt
```

`✓ checkpoint` — le second conteneur répond `can't stat 'greeting.txt': No such file or directory`.

Chaque conteneur reçoit, au-dessus des couches (en lecture seule) de l'image, sa **propre couche inscriptible**. Deux conteneurs issus de la même image partagent les couches du bas, jamais celle du haut. Quand le conteneur est supprimé, sa couche inscriptible l'est aussi.

> Astuce : ce qui suit le nom de l'image dans `docker run` est la commande exécutée dans le conteneur (ici `touch …` puis `stat …`).

## Un volume pour la base SQLite

L'app stocke ses données dans un fichier SQLite : `/etc/todos/todo.db`. Il suffit de faire vivre ce dossier hors du conteneur.

Un **volume nommé** est un « seau » opaque géré par Docker : tu ne retiens que son nom.

```bash
docker volume create todo-db
docker rm -f <id-du-conteneur-actuel>
docker run -dp 127.0.0.1:3000:3000 \
  --mount type=volume,src=todo-db,target=/etc/todos \
  getting-started
```

> Git Bash sous Windows : préfixe le chemin cible d'un `/` supplémentaire (`target=//etc/todos`) pour éviter la conversion de chemin.

Vérifie :
1. ajoute des tâches ;
2. `docker rm -f <id>` ;
3. relance exactement la même commande ;
4. recharge la page.

`✓ checkpoint` — les tâches sont toujours là.

## Où sont mes données ?

```exec
docker volume inspect todo-db
```

Le champ `Mountpoint` (ex. `/var/lib/docker/volumes/todo-db/_data`) est l'emplacement réel sur l'hôte — accessible en root, et dans la VM de Docker Desktop sur Mac/Windows.

`? Q4` — Tu fais `docker rm -f` puis `docker volume rm todo-db`. Que deviennent tes tâches ?
