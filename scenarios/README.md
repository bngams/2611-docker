# Scénario de la formation Docker

Les contenus byte_layers parcourus pendant la formation, rapatriés pour être consultés en local, dans l'ordre de la séance. Chaque dossier contient le cours (`cours/`, le dossier `.layers/` d'origine, avec `index.md` comme point d'entrée) et, quand il existe, le dossier `solution/`.

## Liens de la formation

- Bloc-notes : https://codeshare.io/anynDM
- Tableau blanc : https://excalidraw.com/#room=5f081e3f151ccb61efc2,jhuZF-QpEqqL_sH4p3Oz7w
- Supports et contenu : https://byte-layers.com/fr/stacks/docker
- Exemple de création de conteneur from scratch directement sur Linux : https://github.com/bngams/2512-docker/blob/main/docs/01%20architecture%20conteneurs%20et%20runtimes.md#fonctionnalit%C3%A9s-du-noyau-linux-derri%C3%A8re-les-conteneurs
- Installer Docker sur RHEL : https://docs.docker.com/engine/install/rhel/
- Premières manips Dockerfile : https://byte-layers.com/fr/skills/docker-dockerfile
- Mise en pratique, Docker Workshop : https://byte-layers.com/fr/labs/courses-docker-workshop
- Mise en pratique, scan de config : https://byte-layers.com/fr/labs/scan-image-and-config
- Mise en pratique, server Docker : https://byte-layers.com/fr/labs/compose-nginx-to-traefik/step-1
- Mise en pratique, Java : https://byte-layers.com/fr/labs/java-petclinic-hardened
  - code source du projet de base : https://codeload.github.com/spring-projects/spring-petclinic/tar.gz/500158f732419217507c7656904b8e6aa1bcc0d6

## Les contenus en local

| # | Contenu | Durée | Cours local | En ligne |
|---|---|---|---|---|
| 1 | Premières manips Dockerfile : écrire le Dockerfile d'une application | 2 h | [cours](01-docker-dockerfile/cours/index.md) | [byte-layers.com](https://byte-layers.com/fr/skills/docker-dockerfile) |
| 2 | Mise en pratique : Docker Workshop, de l'image au stack Compose | 3 h 30 | [cours](02-docker-workshop/cours/index.md) | [byte-layers.com](https://byte-layers.com/fr/labs/courses-docker-workshop) |
| 3 | Mise en pratique : scanner un Dockerfile, une image et un fichier Compose | 30 min | [cours](03-scan-image-and-config/cours/index.md) | [byte-layers.com](https://byte-layers.com/fr/labs/scan-image-and-config) |
| 4 | Mise en pratique : server Docker, deux applications derrière nginx, puis Traefik | 1 h 30 | [cours](04-compose-nginx-to-traefik/cours/index.md) · [solution](04-compose-nginx-to-traefik/solution/) | [byte-layers.com](https://byte-layers.com/fr/labs/compose-nginx-to-traefik/step-1) |
| 5 | Mise en pratique : Java, livrer une application sécurisée (Dockerfile, Compose, infra) | 3 h 45 | [cours](05-java-petclinic-hardened/cours/index.md) · [solution](05-java-petclinic-hardened/solution/) | [byte-layers.com](https://byte-layers.com/fr/labs/java-petclinic-hardened) |

Pour le contenu 5, le cours est découpé en trois parties : [Dockerfile](05-java-petclinic-hardened/cours/01-dockerfile/index.md), [Compose](05-java-petclinic-hardened/cours/02-compose/index.md) et [infra](05-java-petclinic-hardened/cours/03-infra/index.md).

## Les réalisations

Les labs 4 et 5 ont été déroulés de bout en bout, et leur résultat est à côté de ce dossier :

- lab 4 : [`../server-compose/`](../server-compose/) ;
- lab 5 : [`../java-petclinic-hardened/`](../java-petclinic-hardened/).

## Lire les cours

Les fichiers sont en Markdown, au format `.layers` de l'extension VS Code [byte_layers](https://marketplace.visualstudio.com/items?itemName=byte-layers.byte-layers). Dans un aperçu Markdown simple, les blocs `exec`, `verify`, `hint` et `solution` s'affichent comme des blocs de code. Chacun annonce son rôle par son étiquette.

Les cours supposent un dossier de travail dans `~/layers-lN/`. Pendant la formation, on a travaillé dans `2611-docker/` à la place.

## Sources

Copie des dépôts [byte-layers/courses-containers](https://github.com/byte-layers/courses-containers) (commit `57fa3fe`) et [byte-layers/courses-docker-workshop](https://github.com/byte-layers/courses-docker-workshop) (commit `a690087`), faite le 6 novembre 2026. Contenus sous licence [CC BY-NC-SA 4.0](LICENSE).
