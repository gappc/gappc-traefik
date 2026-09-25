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

Create the state directory for Traefik's ACME files (account and certificate private keys).
It is ignored by git — never track anything in it:

```
install -d -m 700 state
```

Traefik creates `state/acme.json` / `state/acme-alpn.json` on first start. To keep the state
elsewhere, set `TRAEFIK_STATE_DIR` in `.env`.

Then make sure, that there is a `.htpasswd` file in this directory. This projects traefik configuration won't start without such a file!

If don't have a `.htpasswd`, you can create one using the `htpassed` tool:

```
htpasswd -c .htpasswd YOUR_USERNAME
```

Start docker compose:

```
docker-compose up
```

Traekfik should have started, take a look at [https://traefik.gappc.net/](https://traefik.gappc.net/) and enjoy the dashboard :)

## Security note: `acme.json` was published in this repository

From 2025-11-20 (commit `aa86db7`) to 2026-09-26 this public repository contained a filled
`acme.json`: the Let's Encrypt account key and the private keys of the certificates issued
at that time. It had been tracked as an empty placeholder since 2020, and a routine commit
swept in the live file.

Treated as fully exposed, and made useless rather than hidden (the history is left as is):

- **All 15 certificates in that file expired** between December 2025 and February 2026; none
  of the certificates in use since then shares a key with them.
- **The Let's Encrypt account (`acct/1111528107`) was deactivated** on 2026-09-26, so its key
  can no longer revoke, order or change anything. Traefik registers a fresh account.
- The ACME state moved to `./state/`, which is ignored along with `acme*.json*` and is never
  tracked — the placeholder-file trap that caused this no longer exists.
