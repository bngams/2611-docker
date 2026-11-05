---
title: "Et après ?"
---

Lancer `docker run` ou `docker compose up` à la main en prod ne tient pas : qui relance un conteneur qui meurt ? Comment répartir sur plusieurs machines ? C'est le rôle des **orchestrateurs** — Kubernetes, Swarm, Nomad, ECS — qui maintiennent un *état attendu* (« deux instances de l'app, exposées sur le port 80 ») et corrigent les écarts.

Pistes :
- l'écosystème CNCF (Kubernetes, Prometheus, Envoy, Linkerd, NATS…) ;
- recréer un conteneur « à la main » avec namespaces et cgroups pour démystifier la magie ;
- la suite du parcours byte_layers : `docker-compose-like-a-pro`, puis `kubernetes-first-steps`.
