## Pushpin Dockerfile


This repository contains **Dockerfile** of [Pushpin](http://pushpin.org/) for [Docker](https://www.docker.com/) published to the public [Docker Hub Registry](https://hub.docker.com/).

### Pushpin Version: 1.17.1

### Base Docker Image

* [ubuntu:16.04](https://hub.docker.com/_/ubuntu/)

### Installation

1. Install [Docker](https://www.docker.com/).

2. Download [automated build](https://hub.docker.com/r/fanout/pushpin/) from public [Docker Hub Registry](https://hub.docker.com/): `docker pull fanout/pushpin`

Alternatively, you can build an image from the `Dockerfile`:

```sh
docker build -t fanout/pushpin .
```

### Usage

```sh
docker run -dt -p 7999:7999 --name pushpin --rm fanout/pushpin
```

#### Attach app to accept traffic

By default, Pushpin routes traffic to host "app" port 8080.

1. Start a backend webserver container that exposes port 8080.

2. Start a pushpin container by linking to the backend container:

```sh
docker run -dt -p 7999:7999 --name pushpin --link backend:app fanout/pushpin
```

Open `http://<host>:7999` to see the result.

You can override the target with `-e`. For example:

```sh
docker run -dt -p 7999:7999 --name pushpin --link backend:app -e "target=app:8001" fanout/pushpin
```

#### Attach app to respond to traffic

1. Start a responder container by linking to the pushpin container:

```sh
docker run -d --link pushpin:pushpin ubuntu bash -c "apt-get update; apt-get install -y curl; while true; do curl -s -d '{ \"items\": [ { \"channel\": \"test\", \"formats\": { \"http-stream\": { \"content\": \"hello there\n\" } } } ] }' http://pushpin:5561/publish ; sleep 1; done"
```
