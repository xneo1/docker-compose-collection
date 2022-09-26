<p align="center">
  <a href="https://github.com/xneo1">
    <img src="https://raw.githubusercontent.com/xneo1/docker-compose-collection/master/img/Anchor2.png" width="140px" alt="xneo1" />
  </a>
</p>

<p align="center">
  <a href="https://git.io/typing-svg"><img src="https://readme-typing-svg.herokuapp.com?font=MuseoModerno&size=21&duration=3000&pause=500&multiline=true&width=435&lines=Docker+Container+Collection;for+Portainer+%26+Traefik" alt="Typing SVG" /></a>
</p>

<p align="center">
    <a href="https://github.com/xneo1/docker-compose-collection#list-of-services-availables"><img src="https://img.shields.io/badge/List_of_services-%2341454A.svg?style=for-the-badge&logo=target&logoColor=white"> </a>
    <a href="https://github.com/xneo1/docker-compose-collection#utilisation"><img src="https://img.shields.io/badge/How_to_use-%2341454A.svg?style=for-the-badge&logo=target&logoColor=white"> </a>
    <a href="https://github.com/xneo1/docker-compose-collection#add-new-docker-compose-file"><img src="https://img.shields.io/badge/Add_new_service-%2341454A.svg?style=for-the-badge&logo=target&logoColor=white"> </a>
    <br />
    <img alt="GitHub Workflow Status" src="https://img.shields.io/github/workflow/status/xneo1/docker-compose-collection/CI?label=Files%20generating&logo=files&logoColor=white&style=for-the-badge">
    <br />
    <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/docker-%232496ED.svg?style=for-the-badge&logo=docker&logoColor=white"> </a>
    <a href="https://www.portainer.io/"><img src="https://img.shields.io/badge/portainer-%2313BEF9.svg?style=for-the-badge&logo=portainer&logoColor=white"> </a>
    <a href="https://traefik.io/traefik/"><img src="https://img.shields.io/badge/traefik_proxy-%231F93B1.svg?style=for-the-badge&logo=traefikmesh&logoColor=white"> </a>
    <br />
</p>

<div align="center">
These docker-compose allow you to deploy multiple services easily and quickly.

You can use them with Portainer directly or via docker-compose commands.

All docker-compose are commented and are configured using variables.

They all include support for Traefik.

You can deploy a compatible Docker environment with Portainer and Traefik with:
<p align="center">
  <a href="https://github.com/xneo1/docker-environment"><img src="https://img.shields.io/badge/docker_environment-%2300B8FC.svg?style=for-the-badge&logo=github&logoColor=white"></a>
</p>


## List of services:
##NB_A##

| Status | Service | Website | Update | Maintainer |
##SERVICES##

## List of services to do:
##NB_TD##

| Status | Service |
##SERVICES_TODO##

</div>

---
# How to use
## Portainer
Add this URL in Portainer:
```
https://raw.githubusercontent.com/xneo1/docker-compose-collection/master/templates-portainer.json
```

![PORTAINER](https://github.com/xneo1/docker-compose-collection/blob/master/img/xneo1-docker-compose-collection2.jpg)

## Debian
Install Git :
```bash
 apt install -y git
```

Clone repo
```bash
git clone https://github.com/xneo1/docker-compose-collection/
```


Configuration of variables and execution of a docker-compose:
```bash
cd docker-compose-collection
nano env
sudo docker-compose -f service.yml --env-file env up -d
```
## Some useful commands:

-   **docker container ls** : Show current Docker containers
-   **docker-compose stop** : Stop the containers created with the scripts (in the script folder)
- **docker-compose up -d** : Launch the containers created with the scripts (in the script folder)
-   **docker logs -f <id_container>** : Display the container logs
-   **docker exec -it <id_container> bash** : Get a shell in container

---
# Add new docker-compose file

I automated the creation of the json template file for Portainer and the update of the README.md.

If you want to add a new docker-compose, you must use the following template:
<details>
<summary>Copy the docker-compose template file:</summary>  
  
  
```yaml
# Maintainer: Vagelis Fragkos (xneo1)
# Update: 2022-08-02

#& type: 3
#& title: Hastebin
#& description: Share your code easily
#& note: Website: <a href='https://hastebin.com/about.md' target='_blank' rel='noopener'>Hastebin.com</a>
#& categories: SelfHosted, xneo1
#& platform: linux
#& logo: https://progsoft.net/images/hastebin-icon-b45e3f5695d3f577b2630648bd00584195822e3d.png

#% SERVICE: Name of the service (No spaces or points) [hastebin]
#% DATA_LOCATION: Data location (Example: /apps/service) [/portainer/Files/AppData/Config]
#% URL: Service URL (Example: service.com)
#% NETWORK: Your Traefik network (Example: proxy) [proxy]

# Work with Portainer
version: "2"
services:
  # Hastebin : https://hastebin.com/about.md
  hastebin:
    image: rlister/hastebin:latest
    container_name: $SERVICE
    restart: always
    environment:
      STORAGE_TYPE: file
    volumes:
      - $DATA_LOCATION/$SERVICE/data:/data
    healthcheck:
      test: wget -s 'http://localhost:7777'
      interval: 1m
      timeout: 30s
      retries: 3
    networks:
      - default
    labels:
      - "autoupdate=monitor" # https://github.com/xneo1/container-updater
      - "traefik.enable=true"
      - "traefik.http.routers.$SERVICE.entrypoints=https"
      - "traefik.http.routers.$SERVICE.rule=Host(`$URL`)"
      - "traefik.http.routers.$SERVICE.tls=true"
      - "traefik.http.routers.$SERVICE.tls.certresolver=http"
      - "traefik.docker.network=$NETWORK"

networks:
  default:
    external:
      name: $NETWORK
```
  
</details>
