---
title: "Développer avec un bind mount"
---

## Comparer volume nommé et bind mount

Un **bind mount** partage un dossier *choisi par toi* de l'hôte avec le conteneur. Idéal pour monter du code source : chaque sauvegarde est vue instantanément dans le conteneur.

```text
Volume nommé : type=volume,src=my-volume,target=/usr/local/data
Bind mount   : type=bind,src=/path/to/data,target=/usr/local/data
```

| | Volume nommé | Bind mount |
|---|---|---|
| Emplacement sur l'hôte | choisi par Docker | choisi par toi |
| Pré-rempli avec le contenu du conteneur | oui | non |
| Drivers de volume | oui | non |

## Expérience aller-retour

Depuis `getting-started-app` :

```exec
docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
```

Dans le conteneur :

```exec
cd /src && ls
touch myfile.txt && ls
```

Sur l'hôte : `myfile.txt` est apparu. Supprime-le **depuis l'hôte**, puis refais `ls` dans le conteneur : il a disparu. Quitte avec `Ctrl+D`.

> Docker Desktop en mode Hyper-V : le dossier doit être autorisé dans *Settings => Resources => File sharing*. Inutile en WSL 2.

## Conteneur de développement avec rechargement à chaud

Arrête tout conteneur `getting-started`, puis :

```exec
docker run -dp 127.0.0.1:3000:3000 \
  -w /app --mount type=bind,src="$(pwd)",target=/app \
  node:22-alpine \
  sh -c "npm ci && npm run dev"
```

| Option | Effet |
|---|---|
| `-w /app` | répertoire de travail |
| `--mount type=bind,…` | ton code monté dans `/app` |
| `node:22-alpine` | même base que le Dockerfile |
| `sh -c "…"` | installe les dépendances puis lance `nodemon` (script `dev` du `package.json`) ; Alpine n'a pas `bash` |

```bash
docker logs -f <container-id>   # Ctrl+C pour sortir
```

Attends `Listening on port 3000`.

Dans `src/static/js/app.js` (vers la ligne 109), raccourcis le libellé du bouton :

```diff
- {submitting ? 'Adding...' : 'Add Item'}
+ {submitting ? 'Adding...' : 'Add'}
```

Sauvegarde, rafraîchis.

`✓ checkpoint` — le bouton affiche « Add » sans aucun rebuild. Nodemon a détecté le changement et redémarré le serveur (laisse-lui quelques secondes).

Quand tu as fini : stoppe le conteneur et reconstruis l'image (`docker build -t getting-started .`) pour figer tes changements.

`? Q5` — Pour la base de données en prod, volume ou bind mount ? Pour le code en dev ? Justifie avec le tableau.
