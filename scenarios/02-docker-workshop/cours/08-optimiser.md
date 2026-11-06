---
title: "Optimiser l'image"
---

## Observer les couches

```exec
docker image history getting-started
docker image history --no-trunc getting-started
```

Chaque ligne est une couche : la base en bas, la plus récente en haut, avec sa taille. Chaque instruction du Dockerfile a produit une couche.

## Le piège du cache

Règle : **dès qu'une couche change, toutes les couches suivantes sont reconstruites.**

Avec `COPY . .` avant `RUN npm ci`, la moindre modification d'un fichier source invalide le cache et relance l'installation des dépendances. Absurde.

Réorganise le Dockerfile :

```dockerfile
# syntax=docker/dockerfile:1
FROM node:22-alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev
COPY . .
CMD ["node", "src/index.js"]
```

```exec
docker build -t getting-started .
```

Modifie maintenant le `<title>` de `src/static/index.html` (par ex. « The Awesome Todo App ») et rebuild.

`✓ checkpoint` — les étapes `COPY package.json…` et `RUN npm ci` sont marquées `CACHED` ; le build passe de plusieurs secondes à environ une seconde. Push et pull seront aussi plus rapides.

> Le `.dockerignore` fourni exclut `node_modules` du contexte : sans lui, le `COPY . .` embarquerait tes dépendances locales.

## Builds multi-étapes

Séparer ce qui sert **à construire** de ce qui sert **à exécuter** => images plus petites, moins de surface d'attaque.

Java : le JDK et Maven ne servent qu'à compiler.

```dockerfile
# syntax=docker/dockerfile:1
FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn package

FROM tomcat
COPY --from=build /app/target/file.war /usr/local/tomcat/webapps
```

React : Node ne sert qu'à produire des fichiers statiques, servis ensuite par nginx.

```dockerfile
# syntax=docker/dockerfile:1
FROM node:lts AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY public ./public
COPY src ./src
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

L'image finale = la dernière étape uniquement (modifiable avec `docker build --target <stage>`).

`? Q8` — Dans l'exemple React, que contient l'image finale ? Contient-elle `node_modules` ?
