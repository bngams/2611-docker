---
title: "Mémo commandes"
---

```bash
# images
docker build -t nom .              docker image history nom
docker tag src user/nom            docker push user/nom
# conteneurs
docker run -d -p 127.0.0.1:H:C img docker ps
docker logs -f <id>                docker exec -it <id> sh
docker stop <id> && docker rm <id> docker rm -f <id>
# stockage
docker volume create v             docker volume inspect v
--mount type=volume,src=v,target=/chemin
--mount type=bind,src="$(pwd)",target=/chemin
# réseau
docker network create net          --network net --network-alias alias
# compose
docker compose up -d               docker compose logs -f [svc]
docker compose down [--volumes]    docker compose config
```
