# Knot Docker

> **IMPORTANT**
> This is a community maintained repository, support is not guaranteed.

Docker container and compose setup to run a [Tangled](https://tangled.org) knot
and host your own repository data.

## Pre-built Images

There is a [repository](https://hub.docker.com/r/tngl/knot) of pre-built images
for tags starting at `v1.8.0-alpha` if you prefer.

```
docker pull tngl/knot:v1.10.0-alpha
```

Note that these are *not* official images, you use them at your own risk.

## Building The Image

By default the `Dockerfile` will build the latest tag, but you can change it
with the `TAG` build argument.

```sh
docker build -t knot:latest --build-arg TAG=master .
```

The command above for example will build the latest commit on the `master`
branch.

By default it will also create a `git` user with user and group ID 1000:1000,
but you can change it with the `UID` and `GID` build arguments.

```sh
docker build -t knot:latest --build-arg UID=$(id -u) GID=$(id -g)
```

The command above for example will create a user with the host user's UID and GID.
This is useful if you are bind mounting the repositories and app folder on the host,
as in the provided `docker-compose.yml` file.

<hr style="margin-bottom: 20px; margin-top: 10px" />

When using compose, these can be specified as build arguments which will be
passed to the builder.

```yaml
build:
  context: .
  args:
    TAG: master
    UID: 1000
    GID: 1000
```

This will for example tell docker to build it using the `master` branch like
the command.

## Setting Up The Image

The simplest way to set up your own knot is to use the provided compose file
and run the following:

```sh
export KNOT_SERVER_HOSTNAME=example.com
export KNOT_SERVER_OWNER=did:plc:yourdidgoeshere
export KNOT_SERVER_PORT=443
docker compose up -d
```

This will setup everything for you including a reverse proxy.
