# Traefik configuration for gappc.net

This repo contains a basic [traefik](https://containo.us/traefik/) configuration for the domain [gappc.net](https://gappc.net).

## Getting started

These instructions will get you a copy of the project up and running. For letsencrypt to work your machine must be accessible from the internet under the domain `gappc.net`.

### Prerequisites

To run the project, the following prerequisites must be met:

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [htpasswd](https://httpd.apache.org/docs/2.4/programs/htpasswd.html): to create a basic auth password file for the traefik dashboard. This tool is only necessary if you don't have any `htpasswd` file.

### Installing

Get a copy of the repository, e.g. by cloning it from the following location:

```
git clone git@gitlab.com:gappc/gappc-net-traefik.git
```

Change directory:

```
cd gappc-net-traefik
```

The next step is to make sure, that there is a `.htpasswd` file in this directory. This projects traefik configuration won't start without such a file!

If don't have a `.htpasswd`, you can create one using the `htpassed` tool:

```
htpasswd -c .htpasswd YOUR_USERNAME
```

Start docker compose:

```
docker-compose up
```

Traekfik should have started, take a look at [https://traefik.gappc.net/](https://traefik.gappc.net/) and enjoy the dashboard :)
