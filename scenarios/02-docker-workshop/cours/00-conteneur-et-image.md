---
title: "Conteneur vs image"
---

**Conteneur** : un processus lancé sur la machine hôte, mais isolé des autres processus. L'isolation repose sur deux briques du noyau Linux : les *namespaces* (ce que le processus voit) et les *cgroups* (ce qu'il a le droit de consommer). Docker rend ces mécanismes simples à utiliser.

Un conteneur :
- est une instance exécutable d'une image (on le crée, démarre, arrête, supprime) ;
- tourne aussi bien sur un poste, une VM ou dans le cloud ;
- embarque ses propres binaires, librairies et configuration.

**Image** : le système de fichiers isolé du conteneur, plus ses métadonnées (variables d'environnement, commande par défaut…). Elle doit contenir *tout* ce dont l'application a besoin.

> Analogie : si tu connais `chroot`, un conteneur en est une version étendue — le système de fichiers vient de l'image, et l'isolation va beaucoup plus loin.

`? Q0` — Une image tourne-t-elle ? Un conteneur peut-il exister sans image ?
