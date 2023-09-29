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

## Learned

### Traefik Config with a file

To configure Traefik I chose to use a config file. This gives the optional extra of adding comments to help figure out what I did.

The reverse-proxy container is configured by the [compose](reverse-proxy/compose.yml). In this file there is a volume defined which links to [traefik.yml](reverse-proxy/etc/traefik.yml). I prefer this style of configuration over the commandline variant done in the compose file.

### Explicitly enable traefik for docker image

In every compose file a docker image is explicitly enabled to use traefik with the label: `"traefik.enable=true"`. Default behaviour is exposing every docker image to traefik, this is changed using `exposedByDefault: false` in the traefik.yml

### Global network

The reverse proxy compose file creates a global frontend network so the Traefik Proxy can find them all. Every other compose file includes [common/compose.yml](common/compose.yml) to use this network.

**Beware**: The network has to be created for other containers to start. The easiest way is to start the reverse-proxy first which creates the network.

### File Provider

I want to use Traefik in an existing situation where not all services are provided by docker instances and automatically exposed. Therefor I learned how to have multiple providers to *manually* add other services. In the traefik.yml configuration multiple providers are configured. The file provider will watch the [files.d](reverse-proxy/files.d) directory for services.


## TODO

### Test out Let's Encrypt

This can handle Let's Encrypt, beware to use the staging variant of Let's Encrypt so I don't get banned.

### Enable Dashboard in a secure way.

## Basic Usage

After starting the traefik proxy start another of the services, for instance the my-web.

```bash
cd my-web
docker compose up -d
```

To check if the service is working use curl:
```bash
curl -H Host:my-web.docker.localhost http://127.0.0.1
```


