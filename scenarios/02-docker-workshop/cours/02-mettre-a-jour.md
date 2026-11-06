---
title: "Mettre à jour et cycle de vie"
---

## Modifier le code

Dans `src/static/js/app.js`, repère le texte affiché quand la liste est vide (vers la ligne 56) et remplace-le :

```diff
- <p className="text-center">No items yet! Add one above!</p>
+ <p className="text-center">You have no todo items yet! Add one above!</p>
```

Reconstruis puis relance :

```exec
docker build -t getting-started .
docker run -dp 127.0.0.1:3000:3000 getting-started
```

`⚠ ça casse` — Tu obtiens une erreur du type `Bind for 127.0.0.1:3000 failed: port is already allocated`.

**Pourquoi** : l'ancien conteneur écoute toujours sur le port 3000 de l'hôte. Un seul processus (conteneur compris) peut écouter un port donné.

## Arrêter et supprimer l'ancien conteneur

```bash
docker ps                      # récupère l'ID
docker stop <container-id>
docker rm <container-id>
# ou en une fois :
docker rm -f <container-id>
```

Relance le nouveau conteneur, rafraîchis le navigateur.

`✓ checkpoint` — le nouveau texte s'affiche… et tes tâches précédentes ont disparu. Garde ce constat en tête pour la couche 04.

`? Q2` — Pourquoi reconstruire l'image ne suffit-il pas à mettre à jour l'app qui tourne ?
