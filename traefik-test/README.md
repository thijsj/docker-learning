# Traefik test

Goal: setup traefik proxy as a reverse proxy for several docker images

Site-goals:
 - learn **docker compose**
 - learn how to split up configurations

## Traefik Proxy

Start a Traefik reverse-proxy:

```bash
cd reverse-proxy;
docker compose up -d
```

Learned:
 - configuration op traefik with [traefik.yml](reverse-proxy/etc/traefik.yml). Used it by *mounting* this file as a volume in the [compose](reverse-proxy/compose.yml) on the location `/etc/traefik/traefik.yml`
 - Disabling exposing every docker image to traefik. Explicitly label them to use traefik.
 - Creating a global network.


