---
title: "Docker, couche par couche — de l'image au stack Compose"
description: "Lab Docker pour débutants : conteneuriser une app Node, la publier, persister ses données, la relier à MySQL, la décrire avec Compose et optimiser son image."
tags:
  - lab
  - docker
  - containers
difficulty: beginner
duration: 210
introTitle: "Présentation"
requires: "0.4.0"
options:
  allowManualProgress: true
  autoCompleteOnNext: unverifiable
---

```text
> byte_layers ~/docker/getting-started
>|_ 10 couches · 3h30 · niveau débutant
```

Une image Docker, c'est un empilement de couches. Ce lab aussi : chaque couche s'appuie sur la précédente, et à la fin tu as une app Node + MySQL qui tourne en une commande.

**Fil rouge** : une todo-list en Node.js (`getting-started-app`). Aucune connaissance JavaScript n'est requise.

| Couche | Thème | Durée |
|---|---|---|
| 00 | Conteneur vs image | 10 min |
| 01 | Conteneuriser une app | 25 min |
| 02 | Mettre à jour et cycle de vie | 15 min |
| 03 | Partager via un registry | 20 min |
| 04 | Persister les données (volume) | 20 min |
| 05 | Développer avec un bind mount | 25 min |
| 06 | Multi-conteneurs et réseau | 30 min |
| 07 | Docker Compose | 25 min |
| 08 | Optimiser l'image | 25 min |
| 09 | Et après ? | 5 min |
| ✦ | Défi final | 30 min |

> **Légende**
> `✓ checkpoint` — ce que tu dois observer avant de passer à la suite
> `⚠ ça casse` — une erreur voulue, à comprendre plutôt qu'à contourner
> `? question` — à répondre avant de lire la correction (dernière étape du lab)

**Prérequis** : les bases du shell Linux et `git clone`.

**Ce qu'il faut** : Docker Engine ou Docker Desktop récent (Compose v2 inclus), Git, un éditeur, et un compte Docker Hub pour la couche 03.
