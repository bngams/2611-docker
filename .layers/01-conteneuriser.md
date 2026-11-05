---
title: "Conteneuriser une app"
---

## Récupérer le code

```exec
git clone https://github.com/docker/getting-started-app.git
cd getting-started-app
git checkout 6b025fc5
ls -a
```

On y trouve notamment `package.json`, `package-lock.json`, `src/`, `spec/` et un `.dockerignore`.

## Écrire le Dockerfile

À la racine (à côté de `package.json`), crée un fichier `Dockerfile` (sans extension) :

```dockerfile
# syntax=docker/dockerfile:1
FROM node:22-alpine
WORKDIR /app
COPY . .
RUN npm ci --omit=dev
CMD ["node", "src/index.js"]
EXPOSE 3000
```

| Instruction | Rôle |
|---|---|
| `FROM` | image de base : Alpine légère avec Node et npm |
| `WORKDIR` | dossier courant dans l'image |
| `COPY . .` | copie le code source dans l'image |
| `RUN` | exécuté **au build** : installe les dépendances |
| `CMD` | exécuté **au démarrage** du conteneur |
| `EXPOSE` | documente le port écouté (ne publie rien) |

## Construire l'image

```exec
docker build -t getting-started .
```

- `-t` donne un nom lisible à l'image ;
- `.` est le *contexte de build* : Docker cherche le Dockerfile et les fichiers à copier ici.

Tu vois Docker télécharger plusieurs « layers » : ce sont les couches de `node:22-alpine`, absente de ta machine.

## Lancer un conteneur

```exec
docker run -d -p 127.0.0.1:3000:3000 getting-started
```

- `-d` (`--detach`) : en arrière-plan, le terminal est rendu ;
- `-p HÔTE:CONTENEUR` : publie le port 3000 du conteneur sur `127.0.0.1:3000` de l'hôte. Sans ça, l'app est injoignable depuis ton navigateur.

Ouvre http://localhost:3000, ajoute deux ou trois tâches, coches-en une, supprimes-en une.

```exec
docker ps
```

`✓ checkpoint` — `docker ps` montre un conteneur basé sur `getting-started`, port `3000`, et l'app répond dans le navigateur.

`? Q1` — Que se passe-t-il si tu supprimes la ligne `EXPOSE 3000` mais gardes `-p` ? Et l'inverse ?
