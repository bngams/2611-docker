---
title: "Partager via un registry"
---

## Pousser l'image dans un registry

Pour partager une image, on la pousse dans un **registry**. Par défaut : Docker Hub (c'est de là que vient `node:22-alpine`).

1. Sur Docker Hub, crée un dépôt public nommé `getting-started`.
2. Tente de pousser :

```bash
docker push TON-USER/getting-started
```

`⚠ ça casse` — `An image does not exist locally with the tag: TON-USER/getting-started`.

**Pourquoi** : ton image s'appelle `getting-started`, pas `TON-USER/getting-started`. Le nom d'une image encode *où* elle sera poussée.

3. Connecte-toi, ajoute un second nom à l'image, pousse :

```bash
docker login -u TON-USER
docker tag getting-started TON-USER/getting-started
docker push TON-USER/getting-started
```

Sans tag explicite, Docker utilise `latest`.

## Lancer l'image ailleurs `facultatif`

> Étape optionnelle : à faire si tu as une seconde machine sous la main. Sinon, passe directement à la couche 04.

Sur une machine qui n'a jamais vu l'image (un camarade, une VM, ton VPS) :

```bash
docker run -dp 0.0.0.0:3000:3000 TON-USER/getting-started
```

> **127.0.0.1 vs 0.0.0.0** — `127.0.0.1` n'expose le port que sur l'interface loopback (accès local uniquement). `0.0.0.0` l'expose sur **toutes** les interfaces : la machine devient joignable de l'extérieur. Pratique pour la démo, à manier avec prudence en vrai.

> **Mac Apple Silicon** — ton image est ARM par défaut. Si la machine cible est amd64 :
> `docker build --platform linux/amd64 -t TON-USER/getting-started .`

`✓ checkpoint (facultatif)` — l'image est téléchargée puis l'app répond sur la machine distante.

`✓ checkpoint` — ton image apparaît dans ton dépôt sur Docker Hub, avec le tag `latest`.

C'est exactement le schéma d'un pipeline CI : build => push vers un registry => la prod tire la dernière version.
