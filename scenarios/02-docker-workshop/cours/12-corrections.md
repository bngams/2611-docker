---
title: "Corrections des questions et du défi"
---

**Q0** — Non, une image est un modèle figé (système de fichiers + métadonnées) ; elle ne s'exécute pas. Un conteneur est toujours créé à partir d'une image.

**Q1** — Sans `EXPOSE` mais avec `-p` : ça marche, `EXPOSE` n'est que documentaire (utilisé aussi par `-P`). Avec `EXPOSE` mais sans `-p` : l'app écoute dans le conteneur mais n'est pas joignable depuis l'hôte.

**Q2** — Un conteneur est lié à l'image avec laquelle il a été créé. Il faut en créer un nouveau à partir de la nouvelle image (et libérer le port de l'ancien).

**Q4** — Perdues : le volume contenait le fichier SQLite. Le conteneur, lui, était jetable.

**Q5** — Base en prod : volume nommé (géré par Docker, drivers possibles, indépendant de l'arborescence de l'hôte). Code en dev : bind mount (tu choisis le dossier, modifs instantanées).

**Q6** — Échec de résolution DNS de `mysql` (`getaddrinfo ENOTFOUND mysql` ou équivalent) : l'alias n'existe que sur le réseau `todo-app`, le réseau bridge par défaut n'a pas de DNS par nom.

**Q8** — Uniquement nginx et le dossier `build` (HTML/JS/CSS statiques). Ni Node, ni `node_modules`, ni les sources.

**Défi — piste B1/B2**

```yaml
services:
  app:
    build: .
    ports:
      - 127.0.0.1:3000:3000
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
      MYSQL_DB: todos
    secrets: [db_password]
    depends_on:
      mysql:
        condition: service_healthy

  mysql:
    image: mysql:8.4
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_password
      MYSQL_DATABASE: todos
    secrets: [db_password]
    healthcheck:
      test: ["CMD-SHELL", "mysqladmin ping -h 127.0.0.1 -uroot -p\"$$(cat /run/secrets/db_password)\" --silent"]
      interval: 5s
      timeout: 3s
      retries: 30

secrets:
  db_password:
    file: ./db_password.txt   # à ajouter au .gitignore

volumes:
  todo-mysql-data:
```

Points de vigilance à l'oral :
- `$$` échappe le `$` pour Compose ;
- le fichier secret ne doit pas être commité, et il s'écrit sans retour à la ligne (`printf 'mot-de-passe' > db_password.txt`) : avec `echo`, l'app lit le saut de ligne et MySQL répond `Access denied` ;
- le healthcheck passe par `-h 127.0.0.1` (TCP) plutôt que par le socket local ;
- `127.0.0.1` reste le bon défaut tant qu'un reverse proxy (Traefik, nginx) n'est pas devant.

**Erreurs fréquentes en séance**
- `port is already allocated` => un ancien conteneur tourne encore (`docker ps`).
- `push` refusé => image non taguée avec le namespace, ou `docker login` oublié.
- Bind mount vide sous Windows => partage de fichiers Docker Desktop, ou syntaxe PowerShell (`${PWD}`) / Git Bash.
- `exec format error` sur une autre machine => image ARM lancée sur amd64 (`--platform`).
- App arrêtée sous Compose => MySQL pas prêt au bout de 10 secondes (B1), ou `Access denied` si le fichier secret finit par un saut de ligne.
