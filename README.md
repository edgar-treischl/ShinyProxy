# ShinyProxy Setup

This repository contains all the necessary files to run a ShinyProxy server hosting a (Shiny) app using Docker on an Ubuntu VM.

---

## Prerequisites

- Docker installed on your system.  
  Installation guide: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

- Docker Compose installed (usually comes bundled with Docker Desktop or can be installed separately).

## Repository Contents

- **`application.yml`**  
  Contains ShinyProxy configuration including authentication, app specifications, and Docker network settings.

- **`docker-compose.yml`**  
  Defines services to start ShinyProxy and the Docker network (`shiny-net`), ensuring app containers and ShinyProxy share the same network.

- Your Shiny app images run inside Docker containers, providing isolation, reproducibility, and scalability.


## Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/edgar-treischl/ShinyProxy.git
cd ShinyProxy
```

### 2. Pull Shiny Image (Docker)

```bash
docker pull ghcr.io/edgar-treischl/shinyproxy
```


### 3. Run (For the first time)

```bash
docker network create shiny-net
```


### 4. Start ShinyProxy with Docker Compose

```bash
docker compose up -d
```

This command will start the ShinyProxy container connected to the shiny-net network and make it available on port 8080.


### Add another app 

Adjust the specs to add another app.

```
  specs:
    - id: causalapp
      display-name: Causal App
      description: Application which demonstrates the basics of a Shiny app
      container-image: ghcr.io/edgar-treischl/causalityapp
      container-cmd: ["R", "-e", "shiny::runApp('/app', host='0.0.0.0', port=3838)"]
      container-port: 3838
      logo-url: https://imgs.xkcd.com/comics/correlation_2x.png
```

Please note: The Dockerfile builds an example app provided by openanalytics as a showcase.